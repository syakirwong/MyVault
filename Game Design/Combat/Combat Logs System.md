---
tags:
  - gdd
  - combat
  - ui
---

# Combat Logs System

The combat log is not a record — it is a weapon. Players who read it closely see patterns, anticipate mutations, and exploit information asymmetry. This document defines the log's structure, narrative language, and mechanical rewards.

---

## Design Philosophy

The log serves three audiences simultaneously:

1. **Casual players** — clear summary of what happened ("you dealt 299, took 94")
2. **Attentive players** — tactical data extractable from careful reading (domains used, damage ranges, sequence tendencies)
3. **Expert players** — hidden signals encoded in narrative flavor text (mutation states, resonance proximity, stability thresholds)

The log never lies. But it doesn't volunteer everything either.

---

## Log Structure Per Round

Each round produces four log sections matching the [[Combat System#Round Structure|round phases]].

### Draw Log

Visible only to the owning player. Shows available actions and current state.

```
══ ROUND 7 ══════════════════════════
DRAW ──────────────────────────────
  [1] Force Strike      (Kinetics)
  [2] Storm Strike      (Tempest)
  [3] Corrode           (Entropy)
  [4] Consume           (Void)
  [5] Chain Surge       (Tempest)  ← Momentum bonus draw
──────────────────────────────────
  Momentum: 7/10 ▓▓▓▓▓▓▓░░░
  Stability: K:92 T:85 E:91 V:100
```

**Draw log rules:**
- Always shows gem domain origin for each action
- Momentum-only actions (9+) display with a `[M]` tag
- Desperation actions (0-1) display with a `[D]` tag
- Mutated action variants show the original name struck through: `~~Force Strike~~ → Fracture Pulse (Kinetics)`

### Commit Log

Private during Phase 2. Revealed to both players during Clash.

```
COMMIT ────────────────────────────
  Slot 1: Storm Strike   (Tempest)
  Slot 2: Corrode        (Entropy)
  Slot 3: [IF attacked → Consume (Void)]
          [ELSE → Force Strike (Kinetics)]
──────────────────────────────────
  ▸ LOCKED
```

### Clash Log

The core of the combat log. Both players see identical Clash narration.

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
    ↳ Counter-Energy expended: 94 → 0

  SLOT 3:
  ▸ Opponent attacked (Reflect) → Your conditional triggers: Consume
  ▸ You: Consume → Opponent (absorb Fortify buff remnant)
    ↳ Void absorption: +12 resource
  ▸ Opponent: Construct (damage zone placed, 3 rounds)
──────────────────────────────────
  ROUND RESULT: You dealt 299, took 94
  Integrity: You 78/100 | Opponent 71/100
```

### Aftermath Log

Post-resolution state changes. Mix of mechanical data and narrative signals.

```
AFTERMATH ─────────────────────────
  Mutations:
    Entropy gem: Stability 91 → 87
    Tempest gem: Stability 85 → 82
      ↳ "The storm's edges crackle with unfamiliar rhythm."

  Momentum: 7 → 8 ▓▓▓▓▓▓▓▓░░
    ↳ +1: Won damage exchange

  Echo: Harmonic Strike echo from R6 → Opponent (74 damage)

  Opponent revealed: Aegis domain detected (Fortify used)

  Active constructs:
    Opponent damage zone (2 rounds remaining)
──────────────────────────────────
```

---

## Narrative Signal Language

The log uses specific narrative phrases to encode hidden mechanical information. These are consistent and learnable — players who study them gain a real advantage.

### Mutation Signals

Flavor text appears in the Aftermath log when gems cross stability thresholds.

| Stability Zone | Domain | Example Flavor Text |
|----------------|--------|-------------------|
| Entering Shifting (75) | Kinetics | "The impact wavers, seeking a new shape." |
| Entering Shifting (75) | Tempest | "The storm's edges crackle with unfamiliar rhythm." |
| Entering Shifting (75) | Entropy | "Decay finds a different appetite." |
| Entering Shifting (75) | Void | "The emptiness hums with unexpected resonance." |
| Entering Shifting (75) | Aegis | "The shield remembers a form it hasn't held before." |
| Entering Shifting (75) | Synthesis | "The construct's blueprint blurs, then sharpens differently." |
| Entering Shifting (75) | Flux | "The current bends toward something uncharted." |
| Entering Shifting (75) | Resonance | "The echo returns... changed." |
| Entering Unstable (50) | Any | "Something fundamental shifts within the [domain] gem." |
| Entering Critical (25) | Any | "The [domain] gem fractures — what emerges is barely recognizable." |
| Broken (0) | Any | "The [domain] gem shatters. Silence where its voice once was." |

**Rule:** Flavor text only appears for the **opponent's** gems when the observing player is at Momentum 7+ (Advantage or higher). Below that, opponent mutation is invisible.

### Resonance Signals

Resonance proximity is hinted through escalating language in the Clash log.

| Proximity | Signal | Example |
|-----------|--------|---------|
| 3 rounds away | Ambient hum | "A faint harmonic stirs between your actions." |
| 2 rounds away | Building tension | "The sequence resonates — something is forming." |
| 1 round away | Imminent | "The resonance strains at the edge of release." |
| Triggered | Full narration | "**RESONANCE: Tempest-Entropy — Corrosive Storm** unleashes across the field." |

For opponent resonances, the signal language is vaguer:

| Proximity | Signal |
|-----------|--------|
| 2 rounds away | "You sense a pattern building in the opponent's rhythm." |
| 1 round away | "The opponent's actions hum with aligned purpose." |
| Triggered | Full narration (always visible) |

### Overcharge Signals

```
  ▸ You: Storm Strike [OVERCHARGED] → Opponent (243 Tempest damage)
    ↳ Overcharge: +30% power
    ↳ "The gem screams with borrowed fury."
```

Opponent overcharge (if detected):
```
  ▸ Opponent: [Action] burns with unnatural intensity.
    ↳ Suspected overcharge
```

---

## Information Tiers

What the log reveals depends on **what you've earned**.

### Always Visible

- Your own full draw, commit, stability, momentum
- Both players' committed action names (after Clash)
- All damage numbers dealt and received
- Round result and Integrity totals
- Resonance triggers (both sides)
- Domain of any action used (revealed permanently after first use)

### Earned Through Momentum

| Momentum | Additional Log Detail |
|----------|----------------------|
| 7-8 (Advantage) | Opponent mutation flavor text visible. Your resonance proximity signals appear. |
| 9-10 (Dominance) | Opponent's draw revealed in your log. Opponent stability shown as approximate range (e.g., "~60-70"). Enhanced Clash narration with predicted next-round behavior. |

### Earned Through Prediction

Successful conditional actions (correct branch chosen) unlock:
- **1 correct prediction:** No bonus
- **2 consecutive correct:** Opponent's next Slot 1 action category revealed (Offensive/Defensive/Utility)
- **3 consecutive correct:** Opponent's next full draw partially revealed (2 of 4 actions shown) + 2 momentum bonus

### Never Directly Shown

- Opponent's exact stability numbers (only narrative hints and ranges)
- Opponent's unequipped gem loadout
- Opponent's conditional logic
- Which specific mutation variant an opponent's gem has adopted

---

## Pattern Detection System

The log tracks recurring opponent behaviors and surfaces them as **Counter Prompts** — subtle UI hints that reward attentive play.

### How It Works

The system monitors the last 3-5 rounds of opponent commits and detects:

| Pattern Type | Detection Rule | Counter Prompt |
|-------------|---------------|----------------|
| Repeated opener | Same Slot 1 action 3+ times | "Opponent favors [action] as opener. Counter available." |
| Defensive pivot | Defensive action always follows taking damage | "Opponent tends to guard after pressure. Press or bait?" |
| Burst setup | Specific 2-action sequence used before a high-damage round | "Familiar sequence detected. Brace or disrupt." |
| Domain lock | Only one domain used for 3+ rounds | "Opponent locked into [domain]. Exploit weakness?" |
| Overcharge habit | Overcharge used at consistent Integrity threshold | "Opponent may overcharge soon. Stability target available." |

### Counter Prompt Display

Counter Prompts appear in the Draw phase, below the action list:

```
DRAW ──────────────────────────────
  [1] Force Strike      (Kinetics)
  [2] Consume           (Void)
  [3] Corrode           (Entropy)
  [4] Fortify           (Aegis)
──────────────────────────────────
  ▸ COUNTER: Opponent favors Storm Strike opener.
    Fortify or Consume recommended.
```

**Rules:**
- Counter Prompts are suggestions, never auto-actions
- They appear only when pattern confidence is high (3+ occurrences)
- Expert players can deliberately vary their patterns to avoid triggering opponent counters
- Counter Prompts are themselves a mind-game layer — opponents who know the system can bait false patterns

---

## PvE Log Differences

PvE logs are more generous than PvP to support learning and progression.

| Feature | PvP | PvE |
|---------|-----|-----|
| Enemy intent | Hidden until Clash | Shown as category in Draw phase (Offensive/Defensive/Utility) |
| Enemy stability | Narrative hints only | Visible as percentage bar |
| Mutation signals | Momentum-gated | Always visible |
| Counter Prompts | Pattern-detected only | Supplemented with tactical hints |
| Resonance proximity | Signal language | Explicit countdown ("Resonance in 2 rounds") |
| Boss mechanics | N/A | Dedicated mechanic log line with color coding |

### Boss Mechanic Log Lines

Boss encounters inject special log entries for phase transitions and unique mechanics:

```
CLASH ─────────────────────────────
  !! MECHANIC: Stormclaw Alpha enters Frenzy Phase
     ↳ All Tempest actions gain +50% damage
     ↳ Stability drain doubled on Tempest gems
     ↳ Duration: 5 rounds or until Alpha integrity < 30%
```

### Group PvE Log

In group content, the log is shared. All party members see:

```
CLASH ─────────────────────────────
[R12] Kai commits: Storm Strike → Chain Surge → Overclock
[R12] Mira commits: Fortify → Sanctuary → Construct
[R12] You commit: Corrode → Consume → Destabilize

  SLOT 1:
  ▸ Kai: Storm Strike → Mythic Warden (203 Tempest damage)
  ▸ Mira: Fortify (party-wide 30% reduction)
  ▸ You: Corrode → Mythic Warden (118 Entropy damage, -15% DEF)
    ↳ PARTY RESONANCE: Entropy debuff amplifies Kai's Tempest by +12%
  ▸ Mythic Warden: Seismic Slam → ALL (156 Kinetics damage)
    ↳ Mira's Fortify reduces: 156 → 109
```

Party resonance triggers are highlighted with `PARTY RESONANCE:` tags.

---

## Log History and Review

### In-Combat

- Full scrollable log of all rounds in the current fight
- Round markers for quick navigation: `══ ROUND 7 ══`
- Filterable by: All | Damage | Mutations | Resonance | Mechanics

### Post-Combat Summary

After every fight, a structured summary is generated:

```
══ COMBAT SUMMARY ═════════════════
  Result: VICTORY
  Rounds: 12
  Duration: 3m 42s

  Damage Dealt: 1,847  |  Damage Taken: 1,203
  Momentum Peak: 9 (Dominance)
  Momentum Low: 4 (Neutral)

  Gems:
    Kinetics  — Stable (Stability: 78)
    Tempest   — Shifting (Stability: 62) ← Mutation available
    Entropy   — Stable (Stability: 84)
    Void      — Stable (Stability: 93)

  Resonances Triggered: 2
    R5: Kinetics-Tempest — Stormforce (dealt 187)
    R10: Entropy-Void — Devouring Decay (absorbed 94 + debuff)

  Patterns Detected:
    Opponent favored Aegis opener (7/12 rounds)
    Opponent overcharged at <40% Integrity (2 occurrences)

  Rewards: [See reward panel]
══════════════════════════════════
```

### Combat History Archive

Players can review past fight logs from a dedicated panel (see [[UI Design]]). Stored data:

- Last 50 PvP fights (full log)
- Last 20 dungeon runs (summary + boss logs)
- Bookmarkable rounds within any log
- Exportable as text for the [[Creator Ecosystem]] (build guides, strategy posts)

---

## Log as Content Creation Tool

The combat log feeds directly into the creator ecosystem:

- **Replay sharing** — players can share full fight logs with clickable round navigation
- **Annotated logs** — creators can add commentary to specific rounds ("This is where I read the Aegis pattern and switched to Entropy pressure")
- **Build proof** — log summaries serve as evidence for build guide effectiveness
- **Tournament casting** — enhanced spectator logs show both players' full information for commentary

---

## Related Pages

- [[Combat System]] — Round structure and core mechanics
- [[Gem Identity System]] — Gem domains and resonance
- [[Gem System Math]] — Damage formulas referenced in log numbers
- [[UI Design]] — Panel layout and color language for log display
- [[PvP Structure]] — Competitive modes that generate logs
- [[PvE Structure]] — Dungeon encounters and boss mechanics
- [[Creator Ecosystem]] — How logs feed content creation
