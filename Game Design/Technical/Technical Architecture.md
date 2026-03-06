---
tags:
  - gdd
  - technical
  - architecture
---

# Technical Architecture

## Architecture Decision Record

**Status:** Draft
**Date:** 2026-03-06
**Context:** Stormbound Online is a reactive text-based MMORPG requiring server-authoritative combat, NPC-mediated economy, behavioral identity tracking, gem provenance, and social hubs supporting 200+ concurrent players. This document defines the technical architecture to support these systems at MVP and at scale.

---

## 1. Architecture Overview

### High-Level System Topology

```
                         ┌─────────────────────┐
                         │    Load Balancer     │
                         │   (L7 / WebSocket)   │
                         └──────────┬──────────┘
                                    │
              ┌─────────────────────┼─────────────────────┐
              │                     │                     │
    ┌─────────▼────────┐  ┌────────▼─────────┐  ┌───────▼────────┐
    │   Gateway API    │  │   Gateway API    │  │  Gateway API   │
    │   (REST + WS)    │  │   (REST + WS)    │  │  (REST + WS)   │
    └────────┬─────────┘  └────────┬─────────┘  └───────┬────────┘
             │                     │                     │
    ┌────────▼─────────────────────▼─────────────────────▼────────┐
    │                     Message Bus (NATS)                       │
    └──┬──────────┬──────────┬──────────┬──────────┬──────────┬───┘
       │          │          │          │          │          │
  ┌────▼───┐ ┌───▼────┐ ┌───▼────┐ ┌───▼────┐ ┌───▼────┐ ┌──▼─────┐
  │Combat  │ │Economy │ │Identity│ │ Hub    │ │Match-  │ │Instance│
  │Resolver│ │Service │ │Service │ │Service │ │making  │ │Manager │
  └────┬───┘ └───┬────┘ └───┬────┘ └───┬────┘ └───┬────┘ └──┬─────┘
       │         │          │          │          │          │
  ┌────▼─────────▼──────────▼──────────▼──────────▼──────────▼────┐
  │                     Data Layer                                 │
  │  PostgreSQL (primary)  │  Redis (cache/state)  │  TimescaleDB │
  └────────────────────────────────────────────────────────────────┘
```

### Architectural Decision: Service-Oriented Monolith to Microservices

**Decision:** Start with a **modular monolith** at MVP. Extract into microservices when scale demands it.

**Rationale:** The MVP targets validation of the core gameplay loop with a small player base. A modular monolith with clear service boundaries (combat, economy, identity, hub, matchmaking, instance management) allows rapid iteration without the operational overhead of distributed systems. Each module communicates through internal interfaces that map directly to future service boundaries.

| Optimizes For | Sacrifices |
|---|---|
| Development velocity at MVP | Independent service scaling at launch |
| Reduced operational complexity for a small team | Isolation between failure domains |
| Simpler debugging and tracing | Technology heterogeneity per service |
| Lower infrastructure cost | Deployment independence |

**Extraction triggers** (any one justifies splitting a module into a service):

- Combat resolver CPU usage exceeds 60% of total compute
- Economy service requires independent scaling due to marketplace load
- Hub service needs independent horizontal scaling for 200+ concurrent users per instance
- A module's deployment cadence diverges significantly from others

### Service Boundaries

| Service | Responsibility | State Owned |
|---|---|---|
| **Combat Resolver** | Phase synchronization, action resolution, mutation/stability, momentum, resonance triggers | Active combat sessions |
| **Economy Service** | NPC marketplace, appraisal engine, transaction processing, currency management, crafting | Gold/shard/token balances, marketplace listings, transaction log |
| **Identity Service** | Behavioral axis tracking, archetype detection, cosmetic generation, provenance tracking | Behavioral dimensions, gem provenance, player profiles |
| **Hub Service** | Social hub instances, player lists, chat, build inspection, guild operations | Hub instance state, chat history, guild data |
| **Matchmaking Service** | PvP queue management, ELO/MMR, party formation, dungeon queue | Queue state, rating history |
| **Instance Manager** | Dungeon instance lifecycle, world boss coordination, zone management | Instance registry, zone state |
| **Gateway API** | Authentication, rate limiting, request routing, WebSocket management | Session tokens, connection state |

---

## 2. Client-Server Authority Model

### Authority Distribution

| Domain | Authority | Rationale |
|---|---|---|
| Combat resolution (Clash phase) | **Server** | Anti-cheat. Both commits resolve on the server. Neither client sees the opponent's commit until the server broadcasts the Clash result. |
| Gem state (stability, mutation, provenance) | **Server** | Gem provenance is immutable and must be tamper-proof. Stability changes affect economy (gem breakage = shard generation). |
| Economy (gold, shards, tokens, marketplace) | **Server** | All transactions are ACID. NPC pricing algorithm runs server-side. No client can fabricate currency or items. |
| Behavioral identity (10 axes, archetype) | **Server** | Rolling 50-hour window aggregation must be tamper-proof. Behavioral data influences PvE difficulty and loot affinity. |
| Momentum calculation | **Server** | Momentum thresholds grant mechanical advantages (draw count, speed priority). Client cannot be trusted. |
| Timer enforcement (8-10s Commit) | **Server** | Server starts the timer. Server enforces auto-commit on expiry. Client timer is cosmetic only. |
| UI rendering | **Client** | Text panels, action bars, combat log display, animation timing are client-side. No gameplay state. |
| Input collection | **Client** | Player selects actions, orders sequence, confirms commit. Client sends the commit payload to the server. |
| Combat log formatting | **Client** | Server sends structured event data. Client applies behavioral voice styling, hit effect formatting. |
| Build card display | **Client** | Server provides data. Client renders the build card with cosmetic layers. |

### Why Combat Must Be Server-Authoritative

The phase-based combat system creates a sealed information environment during the Commit phase. Both players select actions without knowledge of the opponent's choices. This design is the foundation of the prediction/counter-prediction skill layer described in [[Combat System#Information Warfare]].

If either client could observe or modify commits before resolution:

1. **Prediction becomes meaningless** -- conditional Slot 3 actions lose all strategic value
2. **Momentum manipulation** -- fabricated round results would corrupt the 0-10 momentum scale
3. **Gem stability fraud** -- clients could prevent stability drain, eliminating the mutation/breakage economy loop
4. **Provenance corruption** -- gem history could be falsified, destroying marketplace trust

The server is the single source of truth for all combat state. Clients are render terminals.

---

## 3. Combat Resolution Server

### Phase Synchronization Protocol

```
CLIENT A                    SERVER                    CLIENT B
   │                          │                          │
   │◄── DRAW (actions) ──────│──── DRAW (actions) ─────►│
   │                          │                          │
   │    [8-10s Commit timer starts on server]            │
   │                          │                          │
   │── COMMIT (sequence) ───►│                          │
   │                          │◄── COMMIT (sequence) ───│
   │                          │                          │
   │    [Server waits for both commits OR timeout]       │
   │                          │                          │
   │◄── COMMIT_ACK ──────────│──── COMMIT_ACK ─────────►│
   │    "Waiting for          │    "Waiting for          │
   │     opponent..."         │     opponent..."         │
   │                          │                          │
   │    [Both commits received. Server resolves.]        │
   │                          │                          │
   │◄── CLASH_RESULT ────────│──── CLASH_RESULT ───────►│
   │    (full narration data) │    (full narration data) │
   │                          │                          │
   │◄── AFTERMATH ───────────│──── AFTERMATH ──────────►│
   │    (mutations, momentum, │    (mutations, momentum, │
   │     echoes, reveals)     │     echoes, reveals)     │
   │                          │                          │
   │    [Next round: DRAW]    │                          │
```

### Commit Phase Rules

1. **Server starts the Commit timer** when it sends DRAW to both clients. The timer value (8-10 seconds, configurable per content type) is included in the DRAW message.
2. **Client timer is advisory.** The client displays a countdown but does not enforce it. The server enforces.
3. **Early commit is buffered.** If Player A commits in 3 seconds and Player B has not committed, the server holds Player A's commit. Player A receives `COMMIT_ACK` with status `waiting`. Player A's commit is not revealed to Player B.
4. **Timeout triggers auto-commit.** If a player has not committed when the server timer expires, the server auto-commits their drawn actions in draw order (first 2-3 actions). The player receives `COMMIT_ACK` with status `auto_committed`.
5. **Both commits received triggers resolution.** The server runs the Clash resolution algorithm synchronously. No partial results are sent.

### Commit Payload Structure

```
CommitPayload {
  match_id:       string       // unique combat session identifier
  round:          integer      // current round number
  player_id:      string       // authenticated player identifier
  sequence:       Action[]     // ordered list of 2-3 selected actions
  slot3_condition: Condition?  // optional conditional for slot 3
  overcharge_gem:  GemSlot?    // optional overcharge target (0-3)
  timestamp:      integer      // client-side timestamp (for latency tracking)
  hmac:           string       // payload integrity hash
}
```

### Clash Resolution Algorithm (Server-Side)

1. Validate both commits against each player's current draw (server-generated draw state)
2. Calculate speed priority per slot (Precision stat + Tempest Chain stacks + Overclock + Momentum bonus)
3. Resolve Slot 1: higher priority resolves first; ties resolve simultaneously
4. Evaluate Slot 3 conditionals against opponent's Slot 1
5. Resolve Slot 2: same priority rules
6. Resolve Slot 3: conditional branch or direct action
7. Apply resonance effects for qualifying sequences
8. Calculate integrity changes
9. Run Aftermath: mutation checks, momentum update, echo resolution, construct ticks, information reveals, overcharge consequences
10. Package results into `CLASH_RESULT` and `AFTERMATH` messages
11. Check win condition (integrity <= 0). If met, send `MATCH_END`.

### Latency Tolerance

| Phase | Latency Budget | Justification |
|---|---|---|
| DRAW delivery | < 200ms | Players waiting for their hand. Perceptible delay is acceptable up to 200ms for a text-based UI. |
| Commit upload | < 500ms | 8-10 second Commit window absorbs network jitter. A 500ms RTT still leaves 7.5-9.5s of decision time. |
| Clash resolution (server compute) | < 100ms | Pure computation. No I/O in the hot path. Pre-loaded combat state in memory. |
| Clash result delivery | < 200ms | Narration text. Players expect a brief pause for "resolution." 200ms is imperceptible against the 3-5 second narration display. |
| Total round overhead (non-player time) | < 700ms | Draw (200) + Clash compute (100) + Result delivery (200) + Aftermath delivery (200). Rounds feel immediate. |

### Timeout Handling

| Scenario | Server Behavior |
|---|---|
| Player does not commit within timer | Auto-commit drawn actions in order. Flag round as `auto_committed` in combat log. |
| Player disconnects during Commit | Start a 15-second reconnection window. If player reconnects, extend Commit timer by 5 seconds. If not, auto-commit. |
| Player disconnects during Clash/Aftermath | Buffer results. Deliver on reconnection. Combat log reconstructs the missed round. |
| Both players disconnect | Pause combat. 60-second reconnection window. If neither returns, match is voided (no rating change, no gem stability loss). |
| One player disconnects and does not return (60s) | Remaining player wins by forfeit. Disconnected player's gems suffer standard post-combat stability assessment. |

### Reconnection Mid-Combat

1. Client reconnects via WebSocket with session token + match ID
2. Server validates session, checks active match state
3. Server sends full combat state snapshot: current round, both players' integrity, momentum zones (approximate for opponent), active constructs, echo queue, gem stability (own gems), and the current phase
4. If reconnecting during Commit phase: remaining timer is sent. Player can still commit if time remains
5. If reconnecting during Clash/Aftermath: buffered results are replayed in sequence
6. Combat log history is sent compressed (last 5 rounds) for context reconstruction

---

## 4. Database Architecture

### Data Store Selection

| Data Type | Store | Rationale |
|---|---|---|
| Gem provenance (full history) | **PostgreSQL** | Immutable append-only lineage. Relational integrity for owner chains, mutation records, resonance history. Provenance is queried by gem ID and must support joins across owners and marketplace transactions. |
| Marketplace transactions | **PostgreSQL** | ACID required. Escrow, spread fee calculation, and gold transfer must be atomic. Foreign keys to gem and player tables enforce referential integrity. |
| Character state | **PostgreSQL** | Gem loadout, inventory, currency balances, stats, alignment, faction reputation. Relational model fits the normalized structure. |
| Guild data | **PostgreSQL** | Membership, permissions, upkeep tracking, recruitment board. Relational. |
| Behavioral identity (rolling window) | **TimescaleDB** (PostgreSQL extension) | Time-series data. 50-hour rolling window aggregation across 10 axes requires efficient windowed queries. TimescaleDB's continuous aggregates handle this natively without custom batch jobs. |
| Active combat sessions | **Redis** | In-memory. Combat state must be read/written at sub-millisecond latency. Matches last 2-12 minutes. Redis TTL handles cleanup. |
| Hub instance state | **Redis** | Player lists, presence, chat buffer. Ephemeral. High read/write frequency. |
| Matchmaking queues | **Redis** (Sorted Sets) | Priority queues with MMR-based sorting. Redis sorted sets provide O(log N) insertion and range queries. |
| Session/auth tokens | **Redis** | Short-lived, high-read. Standard pattern. |
| Economic metrics / dashboards | **TimescaleDB** | Time-series. Gold supply tracking, transaction volume, price history charts (7/30/90-day as specified in [[Trade System#Price History & Transparency]]). Continuous aggregates power the public marketplace API. |

### Schema Considerations

#### Gem Provenance

```
gems
  id              UUID PRIMARY KEY
  domain          ENUM (8 domains)
  rarity          ENUM (Common, Uncommon, Rare, Epic, Legendary)
  owner_id        UUID REFERENCES players(id)
  stability       SMALLINT (0-100)
  mastery         SMALLINT (0-10)
  created_at      TIMESTAMPTZ
  created_by      UUID REFERENCES players(id) NULLABLE  -- crafter, NULL if drop
  source_type     ENUM (drop, crafted, quest_reward)
  mutation_hash   BYTEA  -- cryptographic hash per Economy Stress Test

gem_mutations (append-only)
  id              BIGSERIAL PRIMARY KEY
  gem_id          UUID REFERENCES gems(id)
  mutation_type   TEXT
  stability_at    SMALLINT
  locked_at       TIMESTAMPTZ NULLABLE  -- NULL if reverted
  round_context   JSONB  -- combat state at mutation point

gem_resonances (append-only)
  id              BIGSERIAL PRIMARY KEY
  gem_id          UUID REFERENCES gems(id)
  resonance_id    UUID REFERENCES resonances(id)
  triggered_at    TIMESTAMPTZ
  combat_id       UUID

gem_ownership_log (append-only)
  id              BIGSERIAL PRIMARY KEY
  gem_id          UUID REFERENCES gems(id)
  from_player_id  UUID NULLABLE  -- NULL for initial creation
  to_player_id    UUID
  transaction_id  UUID REFERENCES marketplace_transactions(id) NULLABLE
  transferred_at  TIMESTAMPTZ
```

The append-only tables (`gem_mutations`, `gem_resonances`, `gem_ownership_log`) form the immutable provenance chain described in [[Trade System#Provenance Tracking]]. No UPDATE or DELETE operations are permitted on these tables. Enforced by database-level triggers or row-level security policies.

#### Behavioral Identity

```
behavioral_events (TimescaleDB hypertable, partitioned by time)
  player_id       UUID
  axis            ENUM (10 axes)
  value_delta     NUMERIC
  context         ENUM (pvp, pve, social, economy)
  recorded_at     TIMESTAMPTZ

-- Continuous aggregate: 1-hour buckets per axis per player
behavioral_hourly_agg
  player_id       UUID
  axis            ENUM
  bucket          TIMESTAMPTZ (1-hour)
  weighted_sum    NUMERIC
  event_count     INTEGER

-- Materialized view: current behavioral signature
-- Recalculated from the rolling 50-hour window
-- Updated every 15 minutes via continuous aggregate refresh
behavioral_signature
  player_id       UUID PRIMARY KEY
  aggression      SMALLINT (0-100)
  risk_appetite   SMALLINT (0-100)
  prediction      SMALLINT (0-100)
  momentum_ctrl   SMALLINT (0-100)
  mutation_aff    SMALLINT (0-100)
  resonance_seek  SMALLINT (0-100)
  adaptability    SMALLINT (0-100)
  info_warfare    SMALLINT (0-100)
  content_breadth SMALLINT (0-100)
  social_sig      SMALLINT (0-100)
  primary_arch    TEXT
  secondary_arch  TEXT
  updated_at      TIMESTAMPTZ
```

The 50-hour rolling window uses TimescaleDB's `time_bucket` with a weighted decay function. Newer events weigh more per [[Behavioral Identity#Measurement Rules]]. The continuous aggregate refreshes every 15 minutes -- sufficient granularity given the anti-gaming rule that meaningful shifts require 8-10 hours.

#### Marketplace Transactions

```
marketplace_listings
  id              UUID PRIMARY KEY
  gem_id          UUID REFERENCES gems(id)
  seller_id       UUID REFERENCES players(id)
  appraisal_price BIGINT  -- NPC-calculated midpoint
  listed_price    BIGINT  -- seller-set, constrained to +/-15%
  spread_fee_pct  NUMERIC -- tiered per Trade System (12-20%)
  status          ENUM (active, sold, expired, delisted)
  listed_at       TIMESTAMPTZ
  expires_at      TIMESTAMPTZ  -- +48 hours
  renewed         BOOLEAN DEFAULT FALSE

marketplace_transactions
  id              UUID PRIMARY KEY
  listing_id      UUID REFERENCES marketplace_listings(id)
  buyer_id        UUID REFERENCES players(id)
  sale_price      BIGINT
  spread_fee      BIGINT  -- gold removed from economy
  seller_payout   BIGINT  -- sale_price - spread_fee
  completed_at    TIMESTAMPTZ

-- Enforced via database transaction:
-- 1. UPDATE listing status = 'sold'
-- 2. INSERT marketplace_transaction
-- 3. UPDATE buyer gold balance (deduct sale_price)
-- 4. UPDATE seller gold balance (add seller_payout)
-- 5. UPDATE gem owner_id = buyer_id
-- 6. INSERT gem_ownership_log
-- All six operations in a single SERIALIZABLE transaction.
```

### Relational vs Document Store Decision

**Decision:** PostgreSQL (relational) as the primary store. No document database at MVP.

| Optimizes For | Sacrifices |
|---|---|
| ACID transactions for economy (non-negotiable) | Schema flexibility for rapid prototyping |
| Referential integrity for gem provenance chains | Horizontal write scaling (PostgreSQL scales reads, not writes) |
| Complex queries for marketplace (joins, aggregations, price history) | Document-oriented data patterns (combat log archives) |
| Single operational database to manage | Polyglot persistence optimization |

**Exception:** Combat log archives (historical, read-heavy, append-only) may migrate to a document store or object storage post-MVP if query patterns favor it. At MVP, store as JSONB columns in PostgreSQL.

---

## 5. Scaling Strategy

### Hub Instances (200+ Players)

Per [[Social Hubs]], Port Haven supports 200 players, Sky Forum 300, Thunder Arena 150.

**Approach:** Hub sharding. Each hub is a logical instance managed by the Hub Service. When a hub reaches capacity, a new shard spawns. Players see a unified player list across shards via a shared Redis-backed presence system.

| Component | Scaling Mechanism |
|---|---|
| Player list / presence | Redis pub/sub across hub shards. Each shard publishes join/leave events. Aggregated view served to clients. |
| Chat | Redis Streams. One stream per hub (cross-shard). Clients subscribe to the stream for their current hub. |
| Build inspection | On-demand fetch from Identity Service. Not stored in hub state. |
| NPC panels (marketplace, commission board) | Backed by Economy Service. Hub Service proxies requests. Stateless from the hub's perspective. |

**Scaling trigger:** Hub shard spawns when concurrent connections exceed 80% of capacity. Players are soft-assigned to shards on connection, with friend/guild affinity for social clustering.

### World Bosses (10-50 Players)

Per [[World Design#Verdant Depths]], world bosses support 10-50 players with participant-based scaling.

**Approach:** Dedicated combat instance per world boss spawn. The Instance Manager allocates a combat resolver instance when the boss spawns (2-hour timer). Players join the instance from the open world.

- **Phase synchronization** adapts for N players: during Commit, all players commit independently against the boss. The server resolves all player sequences against the boss's AI sequence.
- **Boss HP and damage scale** with participant count (server-side scaling formula).
- **Instance ceiling:** 50 players. Additional players queue for the next spawn. This is a hard cap to bound combat resolver compute.

### PvP Matchmaking Queues

**Approach:** Redis sorted sets indexed by MMR. Matchmaking runs as a periodic sweep (every 2 seconds) over the queue.

- **1v1 Arena:** Match players within MMR range. Range widens over queue time (starts at +/- 50 MMR, widens by 25 every 15 seconds, caps at +/- 200).
- **3v3 Skirmish:** Party-based queue. Parties matched by average MMR with variance penalty (high-variance parties match against similarly varied groups).
- **Queue from anywhere** per [[World Design#Queue System Integration]]. Player's hub connection remains active. On match found, client receives a match offer. Accept triggers teleport to a combat instance.

### Marketplace Load

The NPC Marketplace targets 30,000+ daily transactions per [[Economy Model#Economic Health Metrics]].

**Approach:** Read-heavy workload. PostgreSQL read replicas serve browse/search queries. Writes (list, purchase, delist) go to the primary.

| Operation | Path |
|---|---|
| Browse/search listings | Read replica + Redis cache (5-second TTL for listing counts, 30-second TTL for price aggregates) |
| Purchase | Primary PostgreSQL, SERIALIZABLE transaction |
| List/delist | Primary PostgreSQL |
| Price history charts | TimescaleDB continuous aggregates, served from read replica |

At 30,000 transactions/day = ~0.35 TPS average, ~3.5 TPS peak (10x burst). PostgreSQL handles this without concern. Scaling becomes relevant at 300,000+ daily transactions (post-MVP growth).

### Dungeon Instances

**Approach:** Ephemeral containers. The Instance Manager maintains a pool of pre-warmed combat resolver containers. When a dungeon party is ready, an instance is allocated from the pool.

- **Instance lifecycle:** Allocate on party ready -> Load dungeon template -> Run combat rounds -> Distribute loot -> Persist results -> Deallocate
- **Pre-warming:** Pool maintains 20% headroom above current demand. Scales based on queue depth.
- **Instance duration:** 10-45 minutes depending on dungeon type (per [[Combat System#Combat Pacing]]). TTL kill at 60 minutes as safety net.

### Scaling Summary

| System | MVP Target | Scale Target | Scaling Method |
|---|---|---|---|
| Concurrent players | 1,000 | 50,000 | Horizontal gateway scaling, service replication |
| Hub instances | 3 hubs, 1 shard each | 5+ hubs, N shards | Hub sharding with Redis presence |
| Active combat sessions | 200 concurrent | 10,000 concurrent | Combat resolver container pool |
| Marketplace TPS | 0.35 avg | 35 avg | Read replicas, write sharding at extreme scale |
| Matchmaking | 100 queued | 5,000 queued | Redis sorted sets scale linearly |

---

## 6. Networking Model

### Protocol Selection

| Protocol | Use Case | Justification |
|---|---|---|
| **WebSocket** | Combat phases, hub presence, chat, real-time notifications | Bidirectional, low-overhead. Combat requires server-push (DRAW, CLASH_RESULT, AFTERMATH). Hub presence requires live player list updates. Text-based game means small payload sizes -- WebSocket framing overhead is negligible. |
| **REST (HTTPS)** | Marketplace browse/search, build inspection, profile queries, crafting operations, inventory management | Request-response pattern. No real-time requirement. Cacheable. Standard tooling for public API (price history, build codes). |
| **Server-Sent Events (SSE)** | Marketplace price alerts, world event announcements, guild notifications | One-way server-to-client. Lighter than WebSocket for fire-and-forget notifications. Fallback for clients that lose WebSocket. |

### Message Format

**Decision:** JSON over WebSocket for MVP. Migrate to MessagePack or Protobuf post-MVP if bandwidth becomes a concern.

| Optimizes For | Sacrifices |
|---|---|
| Human-readable debugging during development | ~30-40% payload size vs binary formats |
| No schema compilation step | Parsing speed (negligible for text-based game payloads) |
| Broad client compatibility | Type safety at the protocol level |

**Payload size estimate for a Clash result:** ~2-4 KB JSON (action names, damage numbers, status effects, narration keys, stability changes, momentum update). For a text-based game, this is trivial. A binary format would reduce to ~1-2 KB -- optimization deferred until player count justifies it.

### Event-Driven Architecture (Internal)

Services communicate internally via **NATS** message bus for async operations:

| Event | Publisher | Subscribers |
|---|---|---|
| `combat.round.completed` | Combat Resolver | Identity Service (behavioral tracking), Economy Service (gem stability updates) |
| `combat.match.ended` | Combat Resolver | Identity Service, Economy Service (gem breakage processing), Matchmaking (rating update) |
| `gem.mutated` | Combat Resolver | Identity Service (mutation affinity tracking), Economy Service (provenance update) |
| `marketplace.transaction.completed` | Economy Service | Identity Service (content breadth tracking) |
| `player.connected` / `player.disconnected` | Gateway | Hub Service (presence), Matchmaking (queue management) |
| `world_boss.spawned` | Instance Manager | Hub Service (announcement), Gateway (notification broadcast) |

Async events decouple services. The Combat Resolver does not wait for Identity Service to record behavioral data before sending the next round's DRAW.

### Connection Lifecycle

```
1. Client connects HTTPS → Gateway API
2. Authenticate (token-based) → session established
3. Upgrade to WebSocket → persistent connection
4. Client subscribes to channels: hub:{hub_id}, match:{match_id}
5. Gateway routes messages to appropriate service
6. On disconnect: Gateway publishes player.disconnected
7. Reconnect: client presents session token, Gateway restores subscriptions
```

---

## 7. State Persistence & Recovery

### Character Checkpointing

| Trigger | What Is Persisted | Target Store |
|---|---|---|
| Logout / disconnect | Full character state (inventory, loadout, currencies, location, quest progress) | PostgreSQL |
| Combat round end | Gem stability changes for the round | Redis (in-memory match state), PostgreSQL (async write-behind) |
| Combat match end | Final gem state (stability, mutations locked/reverted), currency changes, rating changes | PostgreSQL (synchronous, transactional) |
| Marketplace transaction | Gold balances, gem ownership, transaction record | PostgreSQL (SERIALIZABLE) |
| Periodic checkpoint | Incremental character state diff | PostgreSQL (every 5 minutes for active players) |

### Write-Ahead Log for Gem State (Mutation Locks)

Gem mutations during combat are the highest-risk state transition. A mutation lock-in permanently alters a unique asset. This requires journaling.

```
Mutation Lock-In Flow:
1. Player chooses "Lock In" on post-combat mutation prompt
2. Server writes mutation intent to WAL (Redis Stream with AOF persistence)
3. Server executes mutation in PostgreSQL:
   a. UPDATE gems SET stability = new_stability, mutation_hash = new_hash
   b. INSERT INTO gem_mutations (gem_id, mutation_type, stability_at, locked_at, ...)
4. On PostgreSQL commit success: remove WAL entry
5. On PostgreSQL failure: WAL entry remains for retry on next service start
6. WAL entries older than 5 minutes trigger alert (indicates stuck transaction)
```

### Disconnect Recovery Matrix

| Context at Disconnect | Recovery Behavior | Data Guarantee |
|---|---|---|
| In hub (social) | Reconnect returns to same hub shard (or any shard of same hub). No data loss. | Full -- hub state is ephemeral, character state is persisted. |
| In combat (PvP) | 15-second reconnection window. If reconnected: resume combat with state snapshot. If not: auto-commit remaining rounds, forfeit if timer expires. | Full -- combat state in Redis survives client disconnect. |
| In combat (PvE dungeon) | 60-second reconnection window. Party continues (AI takes over disconnected player's commits using auto-commit). Reconnect resumes control. | Full -- dungeon instance persists independently. |
| Mid-marketplace-transaction | Transaction is atomic. Either completed or not. No partial state. | ACID guaranteed. |
| Mid-crafting | Crafting operation is atomic. Materials consumed and item created in single transaction, or neither. | ACID guaranteed. |
| Mid-mutation-lock-in | WAL ensures intent is persisted. Recovery replays the lock-in on reconnect. | WAL + PostgreSQL transaction guarantees. |

### Marketplace Transaction Atomicity

Per the schema in Section 4, marketplace purchases execute as a single SERIALIZABLE PostgreSQL transaction covering six operations. If any operation fails, the entire transaction rolls back. The gem remains listed, the buyer's gold is not deducted, and the seller receives nothing.

**Conflict resolution:** SERIALIZABLE isolation prevents phantom reads. Two buyers attempting to purchase the same listing simultaneously will serialize -- one succeeds, the other receives a "listing no longer available" error.

---

## 8. Security Layer

### Server-Side Validation

Every client message is validated before processing:

| Validation | Scope | Rejection Behavior |
|---|---|---|
| Commit action legitimacy | Actions in commit must exist in the server-generated draw for this round | Reject commit, auto-commit fallback |
| Commit timing | Commit must arrive within the server's timer window (with 500ms network grace) | Reject, auto-commit |
| Gem loadout integrity | Equipped gems must match server-side inventory state | Reject, force re-sync |
| Currency sufficiency | Gold/shard/token balance checked before any deduction | Reject transaction |
| Marketplace price bounds | Listed price must be within +/-15% of NPC appraisal | Reject listing |
| Listing ownership | Only the listing creator can delist | Reject |
| Rate limits | Per-endpoint, per-player rate limits (see below) | 429 Too Many Requests |

### Rate Limiting

| Endpoint Category | Rate Limit | Rationale |
|---|---|---|
| Combat commits | 1 per round per player (enforced by match state) | Structural -- only one commit per round is valid |
| Marketplace browse | 60 requests/minute | Prevents scraping. Generous for normal browsing. |
| Marketplace list/purchase | 10 requests/minute | Prevents automated trading. Normal players never hit this. |
| Chat messages | 20 messages/minute | Anti-spam. Configurable per hub. |
| WebSocket messages (global) | 120 messages/minute | Catch-all for malformed or flood traffic |
| REST API (global) | 300 requests/minute per authenticated user | General abuse prevention |

### Input Sanitization

- All text input (chat, build names, guild descriptions) sanitized server-side
- Combat commits are structured data (enum action IDs, integer slot indices) -- no free-text in combat payloads
- JSONB storage fields are schema-validated before write
- SQL parameterization enforced at the ORM/query builder level (no string concatenation)

### Encryption & Authentication

| Layer | Implementation |
|---|---|
| Transport | TLS 1.3 for all connections (REST and WebSocket) |
| Authentication | JWT tokens with short expiry (15 minutes). Refresh tokens with longer expiry (7 days). Token rotation on refresh. |
| Session binding | WebSocket sessions bound to JWT. Token expiry during active WebSocket triggers transparent refresh (no disconnect). |
| Password storage | bcrypt (cost factor 12) or Argon2id |
| 2FA | TOTP-based. Required for account recovery and device binding. |
| API keys | HMAC-signed keys for the public marketplace API (price trackers, build calculators per [[Trade System#Price History & Transparency]]). |

### DDoS Mitigation

| Layer | Mechanism |
|---|---|
| Edge | Cloud provider DDoS protection (AWS Shield / Cloudflare). Absorbs volumetric attacks before they reach the application. |
| Application | Rate limiting per IP and per authenticated user. WebSocket connection limits per IP (10 concurrent). |
| Protocol | WebSocket ping/pong heartbeat (30-second interval). Connections failing 3 consecutive heartbeats are terminated. |
| Amplification | No unauthenticated endpoints that return large payloads. Marketplace search requires authentication. |

---

## 9. Infrastructure

### Cloud Provider Considerations

**Decision:** Cloud-agnostic containerized deployment. Primary recommendation: **AWS** for MVP due to managed service breadth. Avoid provider lock-in by abstracting cloud-specific services behind interfaces.

| Optimizes For | Sacrifices |
|---|---|
| Managed database (RDS PostgreSQL + TimescaleDB on EC2), managed Redis (ElastiCache), managed container orchestration (EKS) | Cost optimization (managed services cost more than self-managed) |
| Global availability zones for regional deployment | Portability to other clouds without some migration effort |
| Mature monitoring integration (CloudWatch + third-party) | Vendor negotiation leverage |

### Containerization

| Component | Container Strategy |
|---|---|
| Gateway API | Stateless containers behind ALB. Horizontal auto-scaling on CPU/connection count. |
| Combat Resolver | CPU-optimized containers. Pre-warmed pool managed by Instance Manager. Auto-scaling on queue depth. |
| Economy Service | Stateless containers. Moderate scaling -- marketplace load is predictable. |
| Identity Service | Stateless containers. Low scaling pressure -- writes are event-driven, reads are cached. |
| Hub Service | Stateful-ish (WebSocket connections). Scaling via hub sharding, not container replication. |
| PostgreSQL | Managed RDS (primary + read replica). Not containerized. |
| Redis | Managed ElastiCache (cluster mode for hub sharding). Not containerized. |
| TimescaleDB | EC2-hosted or managed Timescale Cloud. Not containerized. |

**Orchestration:** Kubernetes (EKS) for service containers. Helm charts for deployment templating.

### CI/CD Pipeline

```
Code Push → GitHub Actions
  ├── Lint + Type Check
  ├── Unit Tests
  ├── Integration Tests (against test database)
  ├── Security Scan (dependency audit, SAST)
  ├── Build Container Images
  ├── Push to ECR
  ├── Deploy to Staging (automatic)
  ├── Staging Smoke Tests
  └── Deploy to Production (manual approval gate)
```

### Monitoring & Alerting

| Layer | Tool | Metrics |
|---|---|---|
| Infrastructure | CloudWatch + Prometheus | CPU, memory, disk, network per container/instance |
| Application | OpenTelemetry + Grafana | Request latency (p50/p95/p99), error rates, WebSocket connection count, combat rounds/sec |
| Business | Custom dashboards (Grafana) | Economic health metrics per [[Economy Stress Test#Economic Health Dashboard]], concurrent players, matchmaking queue depth, hub population |
| Alerts | PagerDuty / Opsgenie | Combat resolver latency p99 > 500ms, marketplace transaction failure rate > 1%, WebSocket error rate > 5%, gold supply growth > 3%/month |
| Tracing | Jaeger (distributed tracing) | End-to-end request traces across services. Critical for debugging combat resolution timing. |
| Logging | Structured JSON logs -> ELK or CloudWatch Logs | Correlation IDs per combat match, per marketplace transaction |

### Load Balancing

| Traffic Type | Load Balancer | Strategy |
|---|---|---|
| HTTPS (REST) | Application Load Balancer (L7) | Round-robin with health checks |
| WebSocket | Application Load Balancer (L7) with sticky sessions | Connection affinity by player session. Required for hub state consistency. |
| Internal service-to-service | NATS message bus (no load balancer) | NATS handles subscriber distribution |

---

## 10. Performance Targets

### Latency Budgets

| Operation | Target (p95) | Target (p99) | Hard Ceiling |
|---|---|---|---|
| Combat round (Draw to Aftermath delivery) | < 500ms server-side | < 800ms | 1,500ms (triggers investigation) |
| Clash resolution compute | < 50ms | < 100ms | 200ms |
| Marketplace purchase (end-to-end) | < 300ms | < 500ms | 1,000ms |
| Marketplace search | < 200ms | < 400ms | 800ms |
| Hub player list update | < 100ms | < 200ms | 500ms |
| Chat message delivery | < 150ms | < 300ms | 500ms |
| Build card inspection | < 200ms | < 400ms | 800ms |
| Character login (full state load) | < 1,000ms | < 2,000ms | 5,000ms |
| Behavioral signature refresh | < 5,000ms (background, every 15 min) | N/A | N/A |

### Throughput Targets

| Metric | MVP Target | Scale Target |
|---|---|---|
| Concurrent players | 1,000 | 50,000 |
| Concurrent combat sessions | 200 | 10,000 |
| Combat rounds resolved per second | 50 | 2,500 |
| Marketplace transactions per day | 5,000 | 500,000 |
| WebSocket messages per second (aggregate) | 5,000 | 250,000 |
| Behavioral events ingested per second | 100 | 5,000 |

### Concurrent Player Targets

| Milestone | Players | Infrastructure Posture |
|---|---|---|
| **Alpha** | 100 | Single server, modular monolith, single PostgreSQL instance |
| **Closed Beta** | 500 | Modular monolith, PostgreSQL primary + 1 read replica, Redis cluster |
| **MVP Launch** | 1,000-3,000 | Begin service extraction if needed. 2 gateway instances, combat resolver pool of 10. |
| **Growth** | 10,000 | Full microservice architecture. Multi-AZ deployment. 5+ gateway instances. Combat resolver pool of 50. |
| **Scale** | 50,000 | Regional deployment. Database sharding for player data. Dedicated marketplace read replica cluster. Combat resolver pool of 250. |

---

## Trade-Off Summary

| Decision | Optimizes For | Sacrifices |
|---|---|---|
| Modular monolith at MVP | Development speed, simplicity | Independent scaling, fault isolation |
| PostgreSQL as primary store | ACID for economy, referential integrity for provenance | Horizontal write scaling |
| Redis for combat state | Sub-millisecond latency during active matches | Durability (acceptable -- combat is ephemeral) |
| TimescaleDB for behavioral/metrics | Efficient rolling window queries, continuous aggregates | Additional operational component |
| WebSocket for combat | Low-latency bidirectional communication | More complex connection management than REST |
| JSON over WebSocket | Debuggability, development speed | Bandwidth efficiency (~30-40% larger than binary) |
| NATS for internal messaging | Decoupled services, async processing | Added infrastructure component, eventual consistency between services |
| Server-authoritative everything | Anti-cheat, data integrity, trust | Server compute cost, latency sensitivity |

---

## Constraints (Must Not Violate)

1. **Combat commits are never visible to the opponent before both commits are locked.** This is the foundation of the prediction skill layer.
2. **Gem provenance tables are append-only.** No UPDATE or DELETE on `gem_mutations`, `gem_resonances`, `gem_ownership_log`.
3. **Marketplace transactions are ACID and SERIALIZABLE.** No eventual consistency for gold/gem transfers.
4. **Behavioral identity cannot be modified by the client.** All 10 axes are computed server-side from server-observed events.
5. **No direct player-to-player asset transfer exists at any layer.** The NPC intermediation model is structural, not a policy. No API endpoint, no internal service call, no admin tool should enable P2P transfer.
6. **The server is the timer authority for combat Commit phases.** Client timers are cosmetic.
7. **Gem mutation lock-in must survive server failure.** WAL guarantees intent persistence.

---

## Risks

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Combat resolver becomes a bottleneck under load | Medium | High (degrades core gameplay) | Pre-warmed container pool. Horizontal scaling. Stateless design -- any resolver can handle any match. Load test at 2x target concurrency before launch. |
| PostgreSQL write throughput insufficient for marketplace at scale | Low (at MVP), Medium (at 50K players) | Medium | Read replicas absorb search load. At extreme scale, partition marketplace_listings by domain. Consider Citus for horizontal PostgreSQL scaling. |
| Redis data loss during combat (node failure) | Low | Medium (active matches lost) | Redis Cluster with replica failover. Combat matches are 2-12 minutes -- losing a few matches on node failure is acceptable. No permanent state in Redis. |
| WebSocket connection storms after server restart | Medium | Medium (thundering herd) | Exponential backoff with jitter on client reconnect. Gateway connection rate limiting per IP. Gradual rollout on deploys (rolling restart, not full restart). |
| TimescaleDB continuous aggregate lag causes stale behavioral data | Low | Low (behavioral data is inherently delayed -- 50-hour window) | 15-minute refresh interval is well within tolerance. Monitor aggregate lag. Alert at > 30 minutes. |
| NPC pricing algorithm manipulation despite mitigations | Low | Medium (economic distortion) | 30-day volume-weighted average resists short-term manipulation. Delist penalties make wash-listing expensive. Anomaly detection on listing patterns. Quarterly economic review per [[Economy Stress Test#Quarterly Review Checklist]]. |
| Gem duplication exploit | Very Low | Critical (destroys provenance integrity) | Unique gem IDs, mutation hashes, hourly supply audits per [[Economy Stress Test#Scenario 2: Duplication Exploits]]. SERIALIZABLE transactions prevent race conditions. |
| Single region deployment limits geographic reach | Medium | Medium (latency for distant players) | MVP deploys in single region (target: US-East or EU-West depending on beta population). Multi-region deployment planned for Scale milestone. Combat's 8-10 second Commit window tolerates 200ms+ RTT for intercontinental play. |

---

## Dependencies

| Dependency | Type | Risk if Unavailable |
|---|---|---|
| PostgreSQL (primary) | Critical | All persistent state. Total system failure. Mitigated by multi-AZ RDS with automatic failover. |
| Redis | Critical | Combat sessions, hub presence, matchmaking queues halt. Mitigated by Redis Cluster with replicas. |
| NATS | High | Inter-service async communication stops. Behavioral tracking and economic metrics lag. Combat and marketplace continue (synchronous paths). Mitigated by NATS cluster with JetStream persistence. |
| TimescaleDB | Medium | Behavioral signature updates and economic dashboards halt. Core gameplay unaffected. Mitigated by materialized view cache (serves stale data gracefully). |
| Cloud provider (AWS) | Critical | Total outage. Mitigated by multi-AZ deployment. Full cloud migration is a last-resort contingency. |
| TLS certificate authority | Low | New connections fail. Existing connections unaffected. Auto-renewal via ACM/Let's Encrypt. |

---

## Related Pages

- [[GDD Hub]] -- Master game design document index
- [[Combat System]] -- Phase-based combat specification
- [[Gem Identity System]] -- Gem domains, resonance, archetype naming
- [[Economy Model]] -- Currency, source/sink rates, price anchors
- [[Trade System]] -- NPC marketplace, provenance tracking, commission board
- [[Economy Stress Test]] -- Economic failure scenarios and mitigations
- [[Behavioral Identity]] -- 10 behavioral axes, cosmetic generation, rolling windows
- [[World Design]] -- Regions, domain ecology, zone mechanics
- [[Social Hubs]] -- Hub features, capacity, social systems
- [[MVP Design]] -- Scope, success metrics, development priorities
