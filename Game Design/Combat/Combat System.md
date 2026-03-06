---
tags:
  - gdd
  - combat
  - core
---

# Combat System

## Design Philosophy

Combat is a conversation conducted in phases. Each round is a question: *"Given what I know, what I suspect, and what my gems can do right now — what sequence of actions gives me the best outcome against what I think my opponent will do?"*

There are no reflexes. There is no twitch skill. There is reading, predicting, committing, and adapting. The combat log is not a record — it is a weapon. Players who read it closely see patterns, anticipate mutations, and exploit information asymmetry.

Builds are not static. Gems mutate mid-fight. The draw pool shifts. Resonances appear and break. The player who entered the fight is not the player who finishes it.

---

## Round Structure

Each combat round has four phases. A round takes approximately 15-20 seconds. A full PvP fight lasts 8-15 rounds (2-5 minutes).

### Phase 1: Draw (2-3 seconds, automatic)

Each equipped gem generates its current action set based on mutation state. The system draws **4 actions** randomly from the combined pool (~12-16 total).

```
DRAW ──────────────────────────────
  [1] Force Strike      (Kinetics)
  [2] Storm Strike      (Tempest)
  [3] Corrode           (Entropy)
  [4] Consume           (Void)
──────────────────────────────────
  Momentum: 6/10 ▓▓▓▓▓▓░░░░
  Stability: K:92 T:88 E:95 V:100
```

**Momentum modifiers:**
- Momentum 7+: Draw 5 actions instead of 4
- Momentum 9+: Draw includes one **momentum-only action** (see Momentum Actions)
- Momentum 3-: Draw only 3 actions
- Momentum 1: Draw 3, but one is a **Desperation action** (high risk/high reward)

### Phase 2: Commit (8-10 seconds, player input)

Player selects **2-3 actions** from their draw and orders them as a sequence. Order matters — action 1 resolves before action 2.

```
COMMIT ────────────────────────────
  Slot 1: Storm Strike   (Tempest)
  Slot 2: Corrode        (Entropy)
  Slot 3: [CONDITIONAL]
          IF opponent attacks → Consume (Void)
          ELSE → Force Strike (Kinetics)
──────────────────────────────────
  ▸ LOCK IN    ▸ OVERCHARGE gem...
```

**Rules:**
- 2 actions minimum, 3 maximum per commit
- Slot 3 can be a **conditional action** — resolves based on opponent's commit (advanced)
- Unused drawn actions return to the pool
- Timer expires → system auto-commits drawn actions in order

**Overcharge option:** Before locking in, player can choose to **Overcharge** one gem — sacrifice 15 stability for +30% power on that gem's actions this round. Deliberate risk.

### Phase 3: Clash (automatic resolution, ~3-5 seconds narrated)

Both players' sequences resolve simultaneously, action by action.

**Resolution order within each slot:**
1. Speed priority determines which player's Slot 1 resolves first
2. Speed ties resolve simultaneously (both players take effects)
3. After both Slot 1s resolve, both Slot 2s resolve (same priority rules)
4. Conditional Slot 3 evaluates opponent's Slot 1 to determine which branch fires
5. Resonance effects trigger when qualifying sequences complete

**Speed priority** is determined by:
- Base: Precision stat comparison
- Modifier: Tempest Chain stacks add +1 priority per stack
- Modifier: Overclock action grants auto-first for one slot
- Modifier: Momentum 7+ adds +1 priority

```
CLASH ─────────────────────────────
[R7] You commit: Storm Strike → Corrode → Consume
[R7] Opponent commits: Fortify → Reflect → Construct

  SLOT 1:
  ▸ You: Storm Strike → Opponent (187 Tempest damage)
    ↳ Chain stack: 3 → 4
    ↳ Push: Opponent displaced 1 position
  ▸ Opponent: Fortify (50% reduction active)
    ↳ Damage reduced: 187 → 94
    ↳ Counter-Energy stored: +94 CE

  SLOT 2:
  ▸ You: Corrode → Opponent (112 Entropy damage, -15% DEF for 2 rounds)
    ↳ Entropy bonus: debuff extended to 3 rounds
  ▸ Opponent: Reflect → You (94 CE damage)
    ↳ Counter-Energy reset: 94 → 0

  SLOT 3:
  ▸ Opponent used attack (Reflect) → Conditional triggers
  ▸ You: Consume → Opponent (absorb Fortify buff remnant)
    ↳ Void absorption: +12 resource
  ▸ Opponent: Construct (damage zone, 3 rounds)
──────────────────────────────────
  Round result: You dealt 299, took 94
  Integrity: You 78/100 → Opponent 71/100
```

### Phase 4: Aftermath (2-3 seconds)

Post-resolution effects apply:

1. **Mutation check** — gem stability adjusts based on what happened
2. **Momentum update** — calculated from round performance
3. **Echo resolution** — Resonance echoes from previous rounds trigger
4. **Construct tick** — Synthesis constructs activate
5. **Information reveal** — partial data about opponent's loadout revealed
6. **Overcharge consequences** — stability loss applied

```
AFTERMATH ─────────────────────────
  Mutations:
    Entropy gem: Stability 95 → 91 (Corrode used)
    Tempest gem: Stability 88 → 85 (Chain active)

  Momentum: 6 → 7 ▓▓▓▓▓▓▓░░░
    ↳ +1: Won damage exchange
    ↳ Bonus unlocked: Draw 5 next round

  Echo: Harmonic Strike echo from R6 → Opponent (74 damage)

  Opponent revealed: Aegis domain detected (Fortify used)

  Active constructs: Opponent damage zone (2 rounds remaining)
──────────────────────────────────
```

---

## Integrity (Health)

Every combatant has an **Integrity** pool. Reach 0, you lose.

| Factor | Value |
|--------|-------|
| Base Integrity | 100 |
| Resilience bonus | +2 per point of Resilience |
| Aegis domain bonus | +15 base Integrity |
| Typical PvP range | 100-140 |

Integrity does not regenerate naturally. Healing comes from:
- Aegis: Sanctuary (heal from Counter-Energy)
- Synthesis: Healing constructs
- Resonance: Specific resonance effects
- Gems with healing mutation states

---

## Momentum System

Momentum tracks the flow of tactical advantage. It is a 0-10 scale starting at 5.

### Momentum Gain/Loss

| Event | Momentum Change |
|-------|----------------|
| Won damage exchange (dealt more than received) | +1 |
| Landed a resonance effect | +1 |
| Opponent whiffed (action had no valid target) | +1 |
| Successful conditional action (correct prediction) | +2 |
| Lost damage exchange | -1 |
| Your action whiffed | -1 |
| Gem broke (stability reached 0) | -2 |
| Failed conditional (wrong prediction) | -1 |

### Momentum Thresholds

| Level | Effect |
|-------|--------|
| **9-10 (Dominance)** | Draw 5 + momentum-only actions available + combat log shows opponent's draw |
| **7-8 (Advantage)** | Draw 5 + speed priority +1 + resonance trigger chance +15% |
| **4-6 (Neutral)** | Standard: Draw 4, normal rules |
| **2-3 (Pressure)** | Draw 3 + speed priority -1 |
| **0-1 (Desperation)** | Draw 3 + Desperation actions available (high risk/reward, can swing momentum by +3 on success) |

### Momentum is Elastic

Momentum swings are capped at +3 or -3 per round. A single brilliant conditional prediction can swing from Pressure to Advantage. A catastrophic whiff can drop Dominance to Neutral. Fights are never decided until Integrity hits 0.

---

## Momentum-Only Actions

Available only at Momentum 9+. Powerful but fleeting — they disappear if momentum drops.

| Action | Effect |
|--------|--------|
| **Shatter** | Reduce target gem stability by 25. Ignores protection. |
| **Clarity** | Reveal opponent's full draw for 1 round. |
| **Overclock** (enhanced) | Your entire sequence resolves before opponent's. |
| **Resonance Surge** | Force-trigger one resonance effect regardless of conditions. |

## Desperation Actions

Available only at Momentum 0-1. High variance, designed to allow comebacks.

| Action | Effect |
|--------|--------|
| **Last Stand** | Deal damage equal to your missing Integrity. Risk: costs 10 stability on all gems. |
| **Inversion** | Swap your momentum with opponent's. Risk: fails and loses 5 stability if opponent has lower momentum. |
| **Sacrifice** | Destroy one of your gems. Gain +4 momentum and full Integrity heal. Gem is gone for the fight. |

---

## Gem Stability and Mutation

Each gem has a **Stability** meter from 0-100, starting at 100.

### Stability Drain

| Trigger | Stability Loss |
|---------|---------------|
| Using the gem's actions | -2 to -5 per use (varies by action) |
| Taking damage from opposing domain | -5 to -10 |
| Environmental dungeon effects | -3 to -8 per round |
| Opponent's Destabilize action (Entropy) | -15 |
| Overcharge (voluntary) | -15 |
| Shatter (momentum action) | -25 |

### Mutation Thresholds

| Stability | State | Effect |
|-----------|-------|--------|
| 100-76 | **Stable** | Gem operates as designed |
| 75-51 | **Shifting** | Minor mutation: one action in the pool is replaced by a variant. Stats shift ±5% |
| 50-26 | **Unstable** | Moderate mutation: action pool partially randomized. Systemic rule intensity changes (±20%). Resonance pairings may shift |
| 25-1 | **Critical** | Major mutation: action pool heavily altered. Systemic rule may invert. New unexpected resonances may appear. Gem is unpredictable but potentially powerful |
| 0 | **Broken** | Gem ceases to function for the rest of the fight. In high-risk content: gem is permanently destroyed |

### Post-Combat Mutation

After a fight ends, for each gem that mutated (stability < 76):

- **Revert:** Restore gem to original state (free)
- **Lock In:** Permanently adopt the mutation. The gem's base action pool and stats update to the mutated version. This is **gem evolution** — the primary way builds grow and change over time
- **Stabilize:** Pay gold/materials to restore stability without reverting mutations (preserves current state for next fight)

Locking in mutations is how players develop unique gem variants that no one else has. A Kinetics gem that mutated through 50 fights has a different action profile than a fresh one.

---

## Information Warfare

Partial information is core. Knowing what your opponent has is earned, not given.

### What You Know

| Information | Visibility |
|-------------|-----------|
| Your own draw, stability, momentum | Full |
| Opponent's Integrity | Full |
| Opponent's momentum (approximate) | Shown as zone: Desperation / Pressure / Neutral / Advantage / Dominance |
| Opponent's gem domains | Hidden until used; each use reveals that domain |
| Opponent's specific actions | Never shown directly |
| Opponent's stability per gem | Hidden |
| Opponent's draw | Hidden (unless Clarity is used) |
| Opponent's commit | Hidden until Clash |

### Combat Log as Skill

The combat log narrates everything that happens in the Clash phase. Skilled players extract information:

- **Action names** reveal which domain the opponent used
- **Damage numbers** help estimate opponent's stats and gem rarity
- **Sequence patterns** across rounds reveal tendencies (aggressive opener, defensive pivot, burst finisher)
- **Mutation signals** — the log describes when opponent gems shift ("*The edges of the strike fray*" = Entropy mutation)
- **Resonance hints** — the log uses specific language when resonances trigger ("*The echo answers with something new*")

At Momentum 9+, the combat log becomes **enhanced** — it shows richer detail about opponent actions, including approximate stability levels and predicted next-round behavior.

### Log Reading Rewards (Gradient)

| Tier | Mechanism | Reward |
|------|-----------|--------|
| **Implicit** | Player reads log and makes better decisions | Better outcomes (pure skill) |
| **Light explicit** | System detects pattern (opponent used same opener 3 times) | "Counter available" prompt appears in draw |
| **Heavy explicit** | Player successfully predicts conditional action 3 rounds in a row | +2 momentum bonus, opponent's next draw partially revealed |

---

## PvE Combat

Enemies use the same phase system with modifications for readability and fun.

### Enemy Intent

In Phase 1, enemies telegraph partial intent:

```
┌─────────────────────────────────────────┐
│  STORMCLAW ALPHA          HP ████████░░ │
│  ─────────────────────────────────────  │
│  INTENT: [Offensive] → [Offensive]      │
│  DOMAIN: Tempest (revealed)             │
│  STABILITY: ████████░░ ~80%             │
│  HINT: High chain stacks. Expect burst. │
└─────────────────────────────────────────┘
```

- Enemy intent shows **categories** (Offensive / Defensive / Utility), not specific actions
- Enemy domains are revealed after first use (same as PvP)
- Boss stability is visible (adds strategic layer — do you try to break their gems?)
- Hints provide tactical guidance based on game state

### Boss Gem Ecology

Bosses have their own gem loadouts and can mutate, resonate, and break just like players.

- **Boss resonances** are unique — discovering how to trigger or prevent them is part of the encounter design
- **Breaking a boss gem** changes the fight fundamentally (removes actions from their pool, alters their systemic rules)
- **Environmental gem fields** in dungeons buff or nerf specific domains (see [[World Design]])

### Group PvE

No holy trinity. Coordination is emergent.

- Players see each other's commits in Phase 2 before locking in (collaborative sequencing)
- Gem domain synergies between players create **party resonances**:
  - Player A's Kinetics displacement → feeds Player B's Resonance echo zone
  - Player A's Entropy debuff → amplifies Player B's Void Consume damage
  - Player A's Synthesis construct → Player B's Tempest chains off it
- Optimal group composition emerges from domain coverage, not role assignment
- A party of 4 Tempest players CAN work — but lacks resonance diversity

---

## Combat Pacing

| Content | Rounds | Duration | Design Intent |
|---------|--------|----------|---------------|
| PvP Arena (1v1) | 8-15 rounds | 2-5 min | Tactical depth, prediction, momentum swings |
| PvP Skirmish (3v3) | 10-20 rounds | 3-6 min | Team coordination, domain coverage |
| Dungeon Trash | 3-5 rounds | 1-2 min | Fast, test basic loadout efficiency |
| Dungeon Boss | 15-25 rounds | 5-8 min | Extended tactical engagement, mutation management |
| Mythic Boss | 25-40 rounds | 8-12 min | Endurance, adaptation, gem stability under pressure |

---

## Balance Framework

### Power Budget

Every gem loadout has a theoretical power budget of ~100 points distributed across:

| Category | Budget |
|----------|--------|
| Damage output | 25-40 |
| Survivability | 15-30 |
| Control (displacement, debuff, denial) | 10-25 |
| Utility (information, flexibility, constructs) | 10-25 |
| Burst potential | 5-20 |

No single loadout should exceed 100. Loadouts that spike one category sacrifice others.

### Win Rate Targets

- No gem loadout combination should exceed **60% win rate** in PvP across all matchups
- Every loadout should have at least 2 loadouts it's favored against and 2 it's weak against
- Momentum elasticity ensures individual skill can overcome unfavorable matchups

### Combinatorial Space

- 8 domains × 4 slots = **70 possible loadouts**
- 70 loadouts × mutation variants = **thousands of effective builds**
- 28 domain pairs × ~3 resonances = **~80 resonances at MVP**
- Target: **30-50 competitively viable loadouts** at any given time

---

## Related Pages

- [[Gem Identity System]] — Gem domains, resonance, archetype naming
- [[Gem System]] — Gem acquisition, rarity, mastery
- [[Gem System Math]] — Damage formulas and balance calculations
- [[Skill Gems Reference]] — Action sets per gem domain
- [[Modifier Gems Reference]] — Modifier gem interactions
- [[PvP Structure]] — Competitive modes and seasons
- [[PvE Structure]] — Dungeon and encounter design
- [[UI Design]] — Combat panels, log format, enemy intent display
