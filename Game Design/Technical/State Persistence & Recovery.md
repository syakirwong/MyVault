---
tags:
  - gdd
  - technical
  - persistence
---

# State Persistence & Recovery

## Design Philosophy

> **No player should lose meaningful progress to a technical failure.**

All game state is **server-authoritative**. The client is a presentation layer. Every state mutation that matters — gem stability changes, gold transactions, combat outcomes, mutation Lock Ins — is validated, journaled, and committed on the server before the client is notified.

**Journaled writes** are the foundation. State changes are written to a write-ahead log (WAL) before being applied to the primary datastore. If a server crashes mid-operation, the WAL provides a deterministic recovery path: replay incomplete transactions or roll them back. No ambiguous state.

**Optimizes for:** Data integrity, recoverability, auditability.
**Sacrifices:** Write latency (WAL adds ~2-5ms per journaled operation), storage cost (dual-write overhead).

---

## Character State Persistence

### What Is Saved

| State Category | Data | Checkpoint Trigger |
|----------------|------|--------------------|
| Position & region | Current zone, coordinates, facing | Every zone transition + every 30 seconds |
| Inventory | Gold, Shards, Resonance Tokens, Honor, materials, catalysts | On every acquisition or expenditure (transactional) |
| Gem loadout | 4 equipped gems with current stability, mutation state, mastery progress | On every loadout change + post-combat |
| Gem collection | All owned gems with full provenance records | On every gem acquisition, mutation Lock In, or disposal |
| Gem mastery progress | Per-gem mastery XP (0-10 scale) | Post-combat (mastery XP granted in Aftermath phase) |
| Alignment depth | Current alignment + depth value (0-40 MVP) | On every alignment-affecting action |
| Faction reputation | Per-faction standing values | On every reputation-affecting event |
| Quest progress | Active quests, completed objectives, dialogue state | On every objective completion or quest state change |
| Behavioral identity | Archetype name layers, playstyle modifier history | Every 10 combat rounds or on session end |
| Crafting progress | Discipline levels (1-50 x 4), recipe unlocks, crafter reputation | On every craft completion or XP gain |
| Combat log history | Last 50 combat encounters (summary data) | Post-combat |
| Build card state | Published builds, build codes, display preferences | On every publish or edit action |

### Checkpoint Frequency

| Type | Frequency | Scope |
|------|-----------|-------|
| **Micro-checkpoint** | Every state-mutating action | Single field (gold, stability, position) |
| **Session checkpoint** | Every 60 seconds during active play | Full character snapshot |
| **Zone checkpoint** | On every zone transition | Full character snapshot + zone state |
| **Combat checkpoint** | Post-combat (after Aftermath resolves) | Full character + gem state |
| **Hard save** | On logout, disconnect, or server shutdown | Full character + all associated data |

Session checkpoints use delta compression — only changed fields since the last checkpoint are written. Full snapshots occur at zone transitions and combat boundaries.

---

## Combat State

Combat is the highest-risk state for disconnection. Each phase has distinct recovery semantics.

### PvP Disconnect Handling

| Phase | Disconnect Behavior | Rationale |
|-------|---------------------|-----------|
| **Draw** | Auto-proceed with drawn actions. Timer starts for Commit. | Draw is automatic; no player input required. |
| **Commit** | Auto-commit drawn actions in slot order after timer expires (8-10s per [[Combat System]]). | Consistent with existing timeout behavior. Opponent is not penalized. |
| **Clash** | Resolution completes normally using committed actions. | Clash is deterministic server-side. No client input needed. |
| **Aftermath** | Mutations, momentum, echoes resolve normally. | Server-authoritative resolution. |

**Reconnection window:** 30 seconds from disconnect detection.
- If reconnected within 30s: Player resumes at current phase. Missed commits use auto-commit.
- If disconnected > 30s: **Forfeit**. Match result recorded as loss for disconnected player. Opponent receives full victory rewards.
- Gem stability at time of forfeit is preserved (no rollback of mid-fight mutations).
- Momentum and integrity state at disconnect are the final recorded values.
- Repeated disconnects (3+ forfeits in 1 hour) trigger a temporary PvP queue cooldown (15 minutes).

### PvE Solo Disconnect Handling

| Phase | Disconnect Behavior |
|-------|---------------------|
| **Any phase** | Combat **pauses** server-side. No enemy actions resolve. |

- Reconnection window: 5 minutes.
- On reconnect: Player returns to the exact combat state at disconnect. Draw pool, stability, momentum, enemy state — all preserved.
- If disconnected > 5 minutes: Combat instance expires. Player respawns at the **last zone checkpoint** (dungeon entrance or last cleared room). Gem stability reverts to the pre-combat snapshot. No loot awarded.

### PvE Group Disconnect Handling

| Phase | Disconnect Behavior |
|-------|---------------------|
| **Any phase** | **AI takeover** for disconnected player. |

- AI uses a conservative commit strategy: 2 actions per round, no Overcharge, no conditional slots. Domain coverage maintained but no optimization.
- AI takeover lasts **60 seconds**.
- On reconnect within 60s: Player resumes control. AI actions taken during absence are final (stability changes, mutations persist).
- If disconnected > 60s: Character is **removed from instance**. Remaining party members continue with scaled enemy stats (enemies lose HP proportional to removed player's damage contribution).
- Removed player's gem stability reverts to pre-combat snapshot.
- Loot eligibility: Contribution recorded up to disconnect point. If boss dies within 120s of removal, player receives partial loot (scaled by contribution percentage).

### World Boss Disconnect Handling

| Behavior | Detail |
|----------|--------|
| Contribution tracking | Server records damage dealt, debuffs applied, resonances triggered per player per tick (every 5 seconds). |
| On disconnect | Player's contribution is **frozen** at the disconnect point. No further contribution accrues. |
| On reconnect | Player rejoins the fight. New contribution adds to the frozen total. |
| Loot eligibility | Based on total recorded contribution. Minimum threshold: 5% of the player's expected contribution for their gear level. |
| No stability rollback | Gem stability at disconnect is final. World bosses are opt-in high-risk content. |

---

## Gem State Journaling

Gems are the most valuable and complex stateful objects in the game. Every gem mutation is irreversible by design (players choose to Lock In or Revert post-combat). The persistence layer must guarantee that no mutation is lost, duplicated, or applied in an ambiguous state.

### Write-Ahead Log (WAL) for Gem Operations

Every gem state change follows this sequence:

```
1. INTENT recorded to WAL (operation type, gem ID, expected state change)
2. VALIDATION (server checks preconditions: gem exists, stability in range, player owns gem)
3. APPLY (state change written to primary datastore)
4. CONFIRM (WAL entry marked as committed)
```

If the server crashes between steps 1 and 4, recovery reads the WAL:
- **INTENT without CONFIRM:** Roll back. State change never reached primary datastore, or if partially applied, revert to pre-INTENT snapshot.
- **INTENT with CONFIRM:** No action needed. Operation completed successfully.

### Journaled Gem Operations

| Operation | WAL Entry Contains | Rollback Strategy |
|-----------|--------------------|-------------------|
| Stability change (combat use) | Gem ID, previous stability, new stability, cause (action name, round number) | Restore previous stability value |
| Mutation threshold crossing | Gem ID, previous state (Stable/Shifting/Unstable/Critical), new state, action pool changes | Restore previous mutation state + action pool |
| Post-combat Lock In | Gem ID, mutation snapshot (full action pool, stat modifiers, resonance changes) | Restore pre-mutation base gem state |
| Post-combat Revert | Gem ID, confirmation that base state is restored | No rollback needed (revert is the safe state) |
| Post-combat Stabilize | Gem ID, gold/material cost, stability restored to value | Refund gold/materials, restore previous stability |
| Gem breakage (stability 0) | Gem ID, content type (high-risk = permadeath), breakage context | Restore gem to stability 1 (Critical state). Permadeath breakage requires additional confirmation step before WAL commit. |
| Overcharge (voluntary -15 stability) | Gem ID, round number, previous stability | Restore previous stability |

### Mutation Processing Guarantee

During the Aftermath phase, multiple gems may mutate simultaneously. The server processes mutations **sequentially per player, atomically per gem**:

1. Lock: Acquire write lock on player's gem collection.
2. For each gem that crossed a mutation threshold this round:
   - Write INTENT to WAL.
   - Apply mutation (update action pool, stats, resonance eligibility).
   - Write CONFIRM to WAL.
3. Release lock.

If the server crashes mid-sequence, only confirmed mutations persist. Unconfirmed mutations roll back. The player's gem state is always consistent — never partially mutated across multiple gems.

### Provenance Integrity

Every WAL entry for a gem operation is appended to that gem's **provenance record** (see [[Trade System]] for provenance format). The provenance record is append-only and immutable. WAL entries that roll back are recorded as `ROLLED_BACK` in the provenance log — they are visible to GMs but not to players.

---

## Marketplace Transaction Atomicity

All marketplace operations are **ACID-compliant**. The NPC-mediated architecture (see [[Trade System]]) means every transaction involves at least two state changes (e.g., gold deducted from buyer + gem transferred to buyer). These must be atomic.

### Transaction Types

#### Listing a Gem

```
BEGIN TRANSACTION
  1. Validate: Player owns gem, gem not in combat, listing slots available (<15 or <25 for master crafters)
  2. NPC appraisal: Calculate price range (domain, rarity, mutations, mastery, resonance history, crafter signature)
  3. Player sets price within +/-15% of NPC midpoint
  4. Move gem from player inventory to NPC ESCROW
  5. Create marketplace listing record (gem ID, price, expiry 48h, anonymous seller reference)
  6. Deduct listing slot from player's available slots
COMMIT
```

**Failure at any step:** Full rollback. Gem remains in player inventory. No listing created. No slot consumed.

#### Purchasing a Gem

```
BEGIN TRANSACTION
  1. Validate: Listing exists, not expired, buyer has sufficient gold
  2. Deduct gold from buyer (full listed price)
  3. Calculate NPC spread fee (12-20% tiered per Trade System)
  4. Transfer gem from ESCROW to buyer inventory
  5. Credit seller with (listed price - spread fee)
  6. Remove marketplace listing
  7. Update gem provenance record (new owner, purchase date, price)
  8. Destroy spread fee gold (sink)
COMMIT
```

**Failure at any step:** Full rollback. Buyer retains gold. Gem remains in escrow. Listing remains active.

#### NPC Commission Board Transaction

```
BEGIN TRANSACTION
  1. Validate: Commission exists, buyer has materials + fee
  2. Move buyer's materials + fee to NPC ESCROW
  3. Assign commission to crafter (crafter receives work order)
  --- (asynchronous: crafter completes work) ---
  4. Validate: Crafter returns completed item
  5. Transfer completed item from crafter to NPC ESCROW
  6. Transfer completed item from ESCROW to buyer
  7. Transfer fee from ESCROW to crafter (minus 5% NPC commission)
  8. Destroy NPC commission gold (sink)
  9. Update crafter reputation
  10. Update both parties' transaction history
COMMIT
```

Commission transactions span multiple sessions (crafter must complete work). The escrow state is persisted as a **pending transaction** with timeout (72 hours). If the crafter fails to deliver within 72 hours, materials and fee are returned to the buyer automatically.

### Two-Phase Commit for Cross-Service Operations

Operations that span multiple backend services (e.g., inventory service + marketplace service + economy service) use **two-phase commit (2PC)**:

1. **Prepare phase:** Each service validates and locks resources. Reports READY or ABORT.
2. **Commit phase:** If all services report READY, coordinator sends COMMIT to all. If any service reports ABORT, coordinator sends ROLLBACK to all.

Coordinator state is itself journaled. If the coordinator crashes:
- After PREPARE, before COMMIT/ROLLBACK decision: Query all participants for their state. Resume protocol.
- After COMMIT decision sent: Retry COMMIT to any participant that did not acknowledge.
- After ROLLBACK decision sent: Retry ROLLBACK to any participant that did not acknowledge.

**Timeout:** If any participant does not respond within 5 seconds during PREPARE, the transaction ABORTs. Conservative — favors consistency over availability.

---

## Crafting State

### Crafting Session Persistence

A crafting operation (see [[Crafting System]]) involves material input, RNG resolution, and output generation. The server treats each craft as a single atomic transaction.

| Event | State Handling |
|-------|----------------|
| Player initiates craft | Materials are **reserved** (soft lock, not consumed). Craft intent recorded to WAL. |
| RNG resolution | Server determines outcome. Result recorded to WAL before any state change. |
| Output generated | Materials consumed. Output item created. Both operations in single transaction. |
| Disconnect mid-craft | If before RNG resolution: Craft cancelled, materials unreserved. If after RNG resolution but before output: WAL replays on reconnect — player receives the determined output. |

**Key guarantee:** Materials are consumed **only after** the craft result is determined and journaled. A player never loses materials without receiving the corresponding output.

### Material Consumption Atomicity

```
BEGIN TRANSACTION
  1. Validate: Player has required materials, required crafting level, required gold
  2. Record craft intent + RNG seed to WAL
  3. Resolve craft outcome (success/failure, output quality)
  4. Record outcome to WAL
  5. Consume materials from inventory
  6. Consume gold fee
  7. Generate output item (with provenance: crafter signature if applicable)
  8. Add output to player inventory
  9. Award crafting XP
COMMIT
```

Failed crafts (where the crafting system produces a failure outcome) still consume materials minus a small material loss (per [[Trade System]] commission rules). This is an intentional design choice, not a persistence issue. The material loss amount is determined at step 3 and applied at step 5.

### Recipe Unlock Persistence

Recipe unlocks (tied to crafting level thresholds per [[Crafting System]]) are persisted as part of the character state. They are never lost. Recipe unlock state is included in session checkpoints and hard saves.

---

## Death & Defeat Recovery

What happens to player state when integrity reaches 0 depends on content type.

### State Preservation by Content Type

| Content Type | Gem Stability | Buffs/Constructs | Position | Dungeon Progress | Gold/Items |
|-------------|---------------|-------------------|----------|-----------------|------------|
| **PvP Arena** | Preserved (no loss) | Cleared (fight over) | Returns to hub | N/A | No loss. Honor awarded based on performance. |
| **PvP Skirmish (3v3)** | Preserved (no loss) | Cleared | Returns to hub | N/A | No loss |
| **Normal Dungeon** | Mid-fight mutations preserved | Cleared on death | Respawn at dungeon entrance | Room progress preserved. Cleared rooms stay cleared. | No item loss. Gold reward reduced by 25% per death. |
| **Challenge Dungeon** | Mid-fight mutations preserved | Cleared on death | Respawn at last cleared room | Room progress preserved | No item loss |
| **High-Risk Dungeon** | **Gem breakage persisted immediately via WAL** | Cleared on death | Respawn at dungeon entrance. Option to exit. | Room progress preserved if party survives. Lost if full wipe. | Materials dropped on death recoverable from NPC for 50 gold fee |
| **World Boss** | Mid-fight mutations preserved | Cleared on death | Respawn at world boss staging area (30s timer) | Contribution preserved | No loss |

### High-Risk Gem Breakage Persistence

When a gem reaches stability 0 in high-risk content, the breakage is **immediately committed** via the WAL — not deferred to post-combat. This is the only mid-combat permanent state change.

```
1. Stability reaches 0 during Clash resolution
2. WAL INTENT: Gem breakage (gem ID, content type = high-risk, round number)
3. Server validates: Content is indeed high-risk flagged
4. WAL CONFIRM: Gem marked as BROKEN
5. Gem removed from active loadout (actions no longer draw from this gem)
6. Gem converted to BROKEN FRAGMENTS in inventory (salvageable per Crafting System)
7. Player receives Aftermath notification: "[Gem Name] has shattered."
```

This cannot be rolled back by disconnecting. The WAL CONFIRM at step 4 is final. Disconnecting after step 4 means the gem is gone on reconnect. This is by design — high-risk content means high-risk persistence.

**Insurance (Stabilizer catalysts):** Applied before entering the dungeon. Reduces stability drain by 25/50/75% per [[Crafting System]] catalyst tiers. Insurance is consumed on dungeon entry (persisted via WAL), not on breakage. No refund if no breakage occurs.

---

## Backup & Disaster Recovery

### Database Backup Strategy

| Backup Type | Frequency | Retention | Storage |
|-------------|-----------|-----------|---------|
| **WAL continuous archiving** | Continuous (every WAL segment) | 7 days | Local + remote object storage |
| **Incremental backup** | Every 6 hours | 30 days | Remote object storage |
| **Full snapshot** | Daily (during lowest-activity window) | 90 days | Remote object storage + cold archive |
| **Monthly archive** | Monthly | 1 year | Cold archive (glacier-tier) |

### Point-in-Time Recovery (PITR)

WAL continuous archiving enables **point-in-time recovery** to any moment within the 7-day WAL retention window. Granularity: individual transaction.

Use cases:
- Exploit discovered: Roll back affected accounts to pre-exploit state without affecting other players.
- Duplication bug: Identify duplicated gems by provenance hash, remove duplicates, restore correct state.
- Server crash: Replay WAL from last confirmed checkpoint. Maximum data loss: in-flight transactions at crash time (sub-second).

### Cross-Region Replication

| Replication Type | Latency | Purpose |
|------------------|---------|---------|
| **Synchronous replication** | +5-10ms write latency | Primary to secondary within same region. Zero data loss on primary failure. |
| **Asynchronous replication** | 30-60s lag | Primary region to disaster recovery region. Acceptable data loss window for regional failure. |

Failover procedure:
1. Primary region unresponsive for > 60 seconds.
2. Health check confirms failure (not network partition).
3. DR region promoted to primary. Asynchronous lag means up to 60 seconds of transactions may be lost.
4. Affected players notified: "Server recovery in progress. Recent actions in the last 60 seconds may need to be repeated."
5. Original region recovered and demoted to DR. Resynchronization from new primary.

### RTO / RPO Targets

| Metric | Target | Justification |
|--------|--------|---------------|
| **RPO (Recovery Point Objective)** | < 1 second (same-region failure) | Synchronous replication to secondary. |
| **RPO (regional failure)** | < 60 seconds | Asynchronous cross-region replication lag. |
| **RTO (Recovery Time Objective)** | < 5 minutes (same-region failover) | Automated failover to synchronous secondary. |
| **RTO (regional failover)** | < 15 minutes | Manual confirmation required to prevent split-brain. |

---

## Data Migration

### Schema Migration Strategy

Live service schema migrations follow a **expand-contract** pattern:

1. **Expand:** Add new columns/tables alongside existing ones. No breaking changes. Deploy new code that writes to both old and new schema.
2. **Migrate:** Background job backfills new schema from old data. Progress tracked. Restartable if interrupted.
3. **Transition:** New code reads from new schema. Old code paths deprecated but still functional.
4. **Contract:** Once all data migrated and verified, remove old schema. Old code paths removed.

Each phase is a separate deployment. Minimum 24 hours between phases for monitoring.

### Zero-Downtime Deployment

| Requirement | Approach |
|-------------|----------|
| No player-visible interruption | Rolling deployment: new version deployed to subset of servers while old version serves traffic. |
| Database compatibility | Expand-contract ensures old and new code versions can coexist against the same database. |
| Combat in progress | Active combat instances are **never** interrupted. New deployments only route new sessions to updated servers. In-progress combats complete on old version. |
| Marketplace transactions | Pending transactions (escrow, commissions) are version-agnostic. Schema changes never alter in-flight transaction structure. |
| Rollback capability | Previous version remains deployed on standby servers for 24 hours. Instant rollback by rerouting traffic. |

### Feature Flag Integration

New systems and balance changes are gated behind feature flags:

| Flag Type | Scope | Example |
|-----------|-------|---------|
| **Kill switch** | Disable entire feature instantly | `marketplace_v2_enabled: false` |
| **Gradual rollout** | Percentage of players see new feature | `new_mutation_system: 25%` |
| **Region gate** | Feature active only in specific zones | `world_boss_scaling: stormreach_only` |
| **Cohort gate** | Feature active for specific player segments | `crafting_rebalance: beta_testers` |

Flag state is persisted server-side. Clients receive flag evaluations per session. Flag changes propagate within 30 seconds without restart.

---

## Audit Trail

### Logged Actions

All logged actions are stored with: timestamp, player ID, session ID, server ID, action type, payload, and result.

| Category | Logged Actions | Retention |
|----------|---------------|-----------|
| **Gem transactions** | Every marketplace listing, purchase, delist, appraisal. Includes gem ID, price, spread fee, buyer/seller references. | 1 year |
| **Gold transactions** | Every gold gain (source + amount) and gold expenditure (sink + amount). Running balance recorded. | 1 year |
| **Gem mutations** | Every stability change, mutation threshold crossing, Lock In, Revert, Stabilize. Full gem state before and after. | 1 year (summary), 90 days (full state snapshots) |
| **Combat results** | Match type, participants, round count, final integrity, momentum history, gems used, resonances triggered, winner/loser. | 6 months |
| **Crafting operations** | Discipline, recipe, materials consumed, output item, RNG seed, crafter ID, success/failure. | 6 months |
| **Commission board** | Commission creation, acceptance, completion, failure, ratings. Materials in escrow, fees. | 1 year |
| **NPC price updates** | Every appraisal recalculation. Previous price, new price, inputs to pricing formula, volume-weighted average state. | 1 year |
| **Account actions** | Login, logout, password change, 2FA changes, device binding, suspicious login flags. | 2 years |
| **GM/admin actions** | Every GM intervention: item grants, bans, account modifications, economy adjustments. GM ID + justification required. | Permanent |
| **Ban actions** | Ban type (temporary/permanent), reason, evidence references, appeal status, resolution. | Permanent |
| **Gem provenance** | Full creation-to-destruction lifecycle per gem (see [[Trade System]] provenance format). | Permanent (as long as gem or its fragments exist) |
| **Economy aggregates** | Hourly snapshots: total gold supply, total shard supply, marketplace volume, gem population by rarity, breakage count. | Permanent |

### GM Investigation Tools

GMs can query the audit trail by:
- Player ID (all actions by a specific player)
- Gem ID (full lifecycle of a specific gem)
- Transaction ID (specific marketplace or commission transaction)
- Time range (all actions in a window)
- Action type (e.g., all gem breakages in the last 24 hours)
- Anomaly flags (auto-detected: wealth spike > 300% in 24h, duplicate gem hash, > 16h continuous session per [[Economy Stress Test]])

### Retention & Compliance

| Data Class | Retention | Deletion Policy |
|------------|-----------|-----------------|
| Gameplay audit logs | Per table above | Archived to cold storage after retention period. Deleted after 3x retention. |
| Account data | Active account lifetime + 30 days post-deletion request | Hard delete on request (GDPR/privacy compliance). Audit logs anonymized, not deleted. |
| Economy aggregates | Permanent | Never deleted. Essential for long-term economic modeling. |
| GM action logs | Permanent | Never deleted. Accountability record. |

---

## Related Pages

- [[Combat System]] — Phase-based combat, auto-commit on timeout, gem mutation during Aftermath
- [[Gem Identity System]] — Gem domains, stability, mutation thresholds, provenance
- [[Trade System]] — NPC Marketplace, escrow, commission board, provenance tracking
- [[Economy Model]] — Gold sinks/sources, price anchors, economic health metrics
- [[Crafting System]] — Disciplines, material tiers, catalyst consumption
- [[Economy Stress Test]] — Duplication detection, supply audits, bot mitigation
- [[MVP Design]] — MVP scope and development priorities
