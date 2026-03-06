---
tags:
  - gdd
  - combat
  - gems
  - reference
---

# Domain Actions Reference

This document contains the complete action reference for all 8 gem domains at MVP. Each domain contributes 3-4 actions to a player's draw pool. Actions are organized by domain and include base power values, phase-based stats, and alignment variants.

For damage formulas and scaling calculations, see [[Gem System Math]].
For modifier gem options, see [[Modifier Gems Reference]].
For the system overview, see [[Gem System]].
For domain design philosophy and systemic rules, see [[Gem Identity System]].

---

## Reading This Reference

- **Phase**: Which combat phase the action resolves in (see [[Combat System]] for Draw/Commit/Clash/Aftermath structure).
- **Base Power**: Unmodified power value at Mastery 0, Rare rarity. Scales per [[Gem System Math]] mastery formula.
- **Speed Priority Modifier**: Adjusts resolution order within the Clash phase. Higher = resolves first. Base speed priority is 0.
- **Stability Cost**: How much gem stability is consumed per use (see [[Gem Identity System]] for mutation and stability).
- **Alignment Variant**: Behavior change based on the player's [[Alignment System]] commitment. All three variants are listed for every action.

---

## Kinetics

> *"Force ripples outward. The space between you collapses."*

**Systemic Rule:** All your actions have displacement effects. Hits push, misses pull. Positioning determines damage bonuses. The battlefield is never static.

| Action | Base Power | Speed Priority Modifier | Stability Cost | Phase |
|--------|-----------|------------------------|----------------|-------|
| Force Strike | 130 | +0 | 3 | Clash |
| Gravity Well | 90 | -1 | 4 | Clash |
| Momentum Burst | 160 | +1 | 5 | Clash |
| Deflection Field | 100 | +2 | 4 | Clash |

### Force Strike

- **Effect:** Damage + push target 1 position away
- **Phase:** Clash (standard resolution)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Pushed target grants +10% damage bonus to your next action against them |
| Corrupted | Push distance increases to 2 positions; self-takes 5% recoil damage |
| Unbound | If target is already displaced this round, Force Strike costs 0 stability |

### Gravity Well

- **Effect:** Pull target 1 position closer, apply slow (reduced speed priority -1 for 2 rounds)
- **Phase:** Clash (resolves before standard-priority actions)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Slow also reduces target's damage by 10% for 1 round |
| Corrupted | Pull becomes a vacuum that affects all targets in adjacent positions |
| Unbound | Gravity Well can be committed face-down; reveals and resolves in Aftermath instead |

### Momentum Burst

- **Effect:** Damage scales with total displacement this round (base +20% per position displaced by any source)
- **Phase:** Clash (resolves after other Clash actions to maximize displacement scaling)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Excess displacement scaling beyond 60% converts to Integrity healing |
| Corrupted | Base scaling increased to +30% per displacement, but resets all your displacement bonuses afterward |
| Unbound | Momentum Burst counts displacement from the previous round as well as the current |

### Deflection Field

- **Effect:** Redirect next incoming damage to adjacent position (empty or occupied)
- **Phase:** Clash (resolves first due to high speed priority)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Deflected damage is reduced by 25% before redirection |
| Corrupted | Deflected damage is increased by 15% when redirected to a target |
| Unbound | Deflection Field persists for 2 incoming attacks instead of 1, but at 60% effectiveness on the second |

---

## Entropy

> *"The edges of reality fray. Something fundamental gives way."*

**Systemic Rule:** Your actions erode. Everything you touch degrades over time. Debuffs applied by you last 50% longer and stack one tier deeper than normal.

| Action | Base Power | Speed Priority Modifier | Stability Cost | Phase |
|--------|-----------|------------------------|----------------|-------|
| Corrode | 120 | +0 | 3 | Clash |
| Decay Chain | 110 | -1 | 5 | Clash |
| Destabilize | 80 | +1 | 6 | Clash |
| Unravel | 70 | +2 | 4 | Clash |

### Corrode

- **Effect:** Damage + reduce target's defense for 2 rounds (-15% damage reduction)
- **Phase:** Clash (standard resolution)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Defense reduction also applies to target's next outgoing debuff potency |
| Corrupted | Defense reduction increased to -25%, but Corrode's base power reduced by 20% |
| Unbound | If target already has a debuff, Corrode's stability cost is reduced by 2 |

### Decay Chain

- **Effect:** DoT that deals base power over 3 rounds; spreads to second target if unresolved (not cleansed or healed through)
- **Phase:** Clash (resolves late to layer on top of other damage)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Decay Chain can be voluntarily ended early to heal yourself for 30% of remaining DoT value |
| Corrupted | Spread now hits up to 2 additional targets at 60% potency each |
| Unbound | DoT duration extended to 4 rounds but total damage unchanged (lower per-tick, longer pressure) |

### Destabilize

- **Effect:** Increase target gem's instability by 15 (targets a specific equipped gem, chosen randomly if not specified)
- **Phase:** Clash (resolves early to destabilize before other actions trigger mutations)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Destabilize also grants you +5 stability on one of your own gems |
| Corrupted | Instability increased to 20, but one of your own gems also gains +5 instability |
| Unbound | You choose which of the target's gems is destabilized instead of random |

### Unravel

- **Effect:** Strip one active buff from target
- **Phase:** Clash (resolves first to strip buffs before damage actions land)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Stripped buff is converted into a minor heal for you (20% of buff's remaining value) |
| Corrupted | Stripped buff is converted into a debuff on the target (inverse effect, 50% potency, 1 round) |
| Unbound | Unravel strips up to 2 buffs but costs +2 additional stability |

---

## Resonance

> *"The first note of something larger. The echo answers."*

**Systemic Rule:** Your actions echo. Every action produces a diminished copy that repeats 1 round later at 40% power. Repeating the same action type amplifies echoes instead of diminishing them.

| Action | Base Power | Speed Priority Modifier | Stability Cost | Phase |
|--------|-----------|------------------------|----------------|-------|
| Harmonic Strike | 125 | +0 | 3 | Clash |
| Amplify | 0 | +2 | 4 | Commit |
| Frequency Shift | 0 | +3 | 5 | Clash |
| Dampening Wave | 100 | +1 | 5 | Clash |

### Harmonic Strike

- **Effect:** Damage + place echo marker on target (echo repeats at 40% power next round)
- **Phase:** Clash (standard resolution)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Echo marker also heals you for 15% of the echo's damage when it triggers |
| Corrupted | Echo power increased from 40% to 55%, but echo also damages you for 10% of its value |
| Unbound | Echo marker persists for 2 rounds instead of 1 (triggers twice at diminishing power: 40%, then 20%) |

### Amplify

- **Effect:** Double the power of your next echo (applies to the next echo that triggers, regardless of source action)
- **Phase:** Commit (must be committed before Clash; takes effect when the next echo resolves)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Amplified echo also grants +1 speed priority on your next round |
| Corrupted | Amplification is 2.5x instead of 2x, but the amplified echo costs 3 additional stability |
| Unbound | Amplify can target a specific pending echo instead of the next one to trigger |

### Frequency Shift

- **Effect:** Move all pending echoes forward (trigger immediately at current power) or backward (delay 1 additional round, gain +10% power per delay)
- **Phase:** Clash (resolves before echoes trigger due to high speed priority)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Forward shift: echoes also apply a minor slow (-1 speed priority for 1 round) to targets hit |
| Corrupted | Backward shift: power gain increased to +20% per delay, but max delay capped at 2 rounds |
| Unbound | Frequency Shift can be split: some echoes forward, some backward (player chooses per echo) |

### Dampening Wave

- **Effect:** Cancel all echoes and buffs on target (yours and theirs); deals base power damage per echo cancelled
- **Phase:** Clash (resolves after echo-generating actions to maximize cancellation count)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Cancelled echoes heal you for 10% of their stored power each |
| Corrupted | Damage per cancelled echo increased by 50%, but your own pending echoes are also cancelled |
| Unbound | Dampening Wave only cancels the target's echoes and buffs, leaving yours intact |

---

## Void

> *"The void remembers nothing. Where the strike lands, nothing remains."*

**Systemic Rule:** Your actions negate. You can consume opponent's buffs to fuel your own damage, create dead zones where actions fail, and temporarily remove yourself from existence.

| Action | Base Power | Speed Priority Modifier | Stability Cost | Phase |
|--------|-----------|------------------------|----------------|-------|
| Consume | 115 | +0 | 4 | Clash |
| Null Zone | 0 | -1 | 7 | Clash |
| Siphon | 90 | +1 | 5 | Clash |
| Absence | 0 | +3 | 6 | Clash |

### Consume

- **Effect:** Damage + absorb one buff from target (converts to temporary resource: +10% damage on next action)
- **Phase:** Clash (standard resolution)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Consumed buff is purified; grants you a shield equal to 15% of Consume's damage |
| Corrupted | Consumed buff is corrupted; the +10% damage bonus becomes +20% but lasts only 1 action |
| Unbound | If target has no buffs, Consume still deals full damage and refunds 2 stability |

### Null Zone

- **Effect:** Create a zone for 2 rounds where all committed actions fail (no damage, no effects, no echoes)
- **Phase:** Clash (resolves late; zone activates on the following round)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Allies in Null Zone gain +10% damage reduction from sources outside the zone |
| Corrupted | Null Zone also drains 5 stability per round from all gems of anyone inside (including you) |
| Unbound | Null Zone duration reduced to 1 round but can be placed reactively in Aftermath phase |

### Siphon

- **Effect:** Steal 20% of target's current resource pool (Integrity, stored CE, Chain stacks, or construct count -- targets the highest-value resource)
- **Phase:** Clash (resolves early to steal before target spends resources)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Siphoned resources are shared with the lowest-resource ally (in group content) |
| Corrupted | Steal increased to 30%, but you take 5% of the stolen amount as self-damage |
| Unbound | You choose which resource to siphon instead of automatic highest-value targeting |

### Absence

- **Effect:** Become untargetable for 1 phase, but cannot act during that phase
- **Phase:** Clash (resolves first; you are removed from targeting for the remainder of Clash)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Returning from Absence grants +2 speed priority on your next round |
| Corrupted | Absence duration extends to 2 phases, but costs double stability |
| Unbound | During Absence, you can still observe opponent's committed actions (information advantage) |

---

## Flux

> *"It was fire, then it was ice, then it was something without a name."*

**Systemic Rule:** Your actions transform. Damage type, target count, and secondary effects shift based on context -- specifically, the opponent's last action determines what your next action becomes. You are inherently unpredictable.

| Action | Base Power | Speed Priority Modifier | Stability Cost | Phase |
|--------|-----------|------------------------|----------------|-------|
| Shift Strike | 135 | +0 | 3 | Clash |
| Metamorphosis | 0 | +1 | 6 | Aftermath |
| Adaptive Shield | 110 | +2 | 4 | Clash |
| Phase Walk | 0 | +3 | 4 | Clash |

### Shift Strike

- **Effect:** Damage; type and bonus effect mirrors opponent's last action (if opponent used a damage action, Shift Strike gains +15% damage; if defensive, Shift Strike gains minor self-healing; if utility, gains speed priority bonus)
- **Phase:** Clash (standard resolution)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Mirrored bonus effect is 25% stronger and also applies to your next action |
| Corrupted | Shift Strike always mirrors as damage regardless of opponent's action type, at +20% power |
| Unbound | Shift Strike can mirror any action from the last 2 rounds, not just the most recent |

### Metamorphosis

- **Effect:** Replace one gem's current action set with a random alternative from that domain (the gem's systemic rule remains, but available actions change)
- **Phase:** Aftermath (takes effect at the start of next round)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Metamorphosis grants +10 stability to the transformed gem |
| Corrupted | You choose the replacement action set instead of random, but the gem loses 10 stability |
| Unbound | Metamorphosis can be reversed on the following round at no stability cost |

### Adaptive Shield

- **Effect:** Block; resistance type matches whatever damage element is incoming (if Storm damage, gains Storm resistance; if Void, gains Void resistance, etc.)
- **Phase:** Clash (resolves first due to high speed priority)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Adaptive Shield also grants the matched resistance to one ally for 1 round |
| Corrupted | Shield absorbs 20% more but shatters into AoE damage (30% of absorbed) if fully depleted |
| Unbound | Adaptive Shield adapts to 2 damage types simultaneously if hit by mixed-element attacks |

### Phase Walk

- **Effect:** Reposition unpredictably; opponent cannot target you with their next positional action
- **Phase:** Clash (resolves first; repositioning occurs before damage resolution)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Phase Walk leaves a decoy at your original position that absorbs one action |
| Corrupted | Phase Walk deals 40% base power damage to anyone adjacent to your arrival position |
| Unbound | Phase Walk grants +1 speed priority for 2 rounds after repositioning |

---

## Tempest

> *"The storm builds. One, two, three -- the rhythm becomes a weapon."*

**Systemic Rule:** Your actions chain. Landing hits builds Chain stacks (max 5). Each stack increases your speed priority and adds bonus damage. Missing an action resets ALL stacks to zero.

| Action | Base Power | Speed Priority Modifier | Stability Cost | Phase |
|--------|-----------|------------------------|----------------|-------|
| Storm Strike | 120 | +0 | 3 | Clash |
| Lightning Chain | 140 | +0 | 4 | Clash |
| Overclock | 0 | +5 | 5 | Commit |
| Surge | 180 | -2 | 6 | Clash |

### Storm Strike

- **Effect:** Damage + gain 1 Chain stack
- **Phase:** Clash (standard resolution)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | At 3+ Chain stacks, Storm Strike also grants +5% damage reduction for 1 round |
| Corrupted | Storm Strike gains +10% damage per Chain stack but costs +1 stability per stack held |
| Unbound | Storm Strike generates 2 Chain stacks if it is the first action committed this round |

### Lightning Chain

- **Effect:** Damage; bonus damage equal to 15% per Chain stack held
- **Phase:** Clash (standard resolution)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Lightning Chain does not consume Chain stacks and adds a minor slow to target |
| Corrupted | Bonus per stack increased to 20%, but Lightning Chain consumes 1 Chain stack on use |
| Unbound | Lightning Chain can split damage across 2 targets at 60% effectiveness each |

### Overclock

- **Effect:** Your next action resolves first regardless of speed priority
- **Phase:** Commit (committed before Clash; effect applies to your next Clash action)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Overclock also grants +1 Chain stack when the overclocked action resolves |
| Corrupted | Overclocked action deals +25% damage but costs 8 stability instead of 5 |
| Unbound | Overclock applies to your next 2 actions instead of 1, but cannot be used again for 2 rounds |

### Surge

- **Effect:** Spend all Chain stacks; deal burst damage equal to base power + 30% per stack consumed
- **Phase:** Clash (resolves late to maximize stack count from other actions this round)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Surge refunds 1 Chain stack if it reduces target below 50% Integrity |
| Corrupted | Damage per stack increased to 40%, but Surge also costs Integrity equal to 5% per stack spent |
| Unbound | Surge retains 1 Chain stack after spending (does not fully reset) |

---

## Aegis

> *"The shield holds. What didn't kill you is about to kill them."*

**Systemic Rule:** Your actions reflect. Damage blocked or absorbed generates Counter-Energy (CE). CE is your offensive resource -- the more you endure, the harder you hit back. Defense IS offense.

| Action | Base Power | Speed Priority Modifier | Stability Cost | Phase |
|--------|-----------|------------------------|----------------|-------|
| Fortify | 0 | +2 | 3 | Clash |
| Reflect | 150 | +0 | 4 | Clash |
| Endure | 0 | +3 | 5 | Clash |
| Sanctuary | 0 | +1 | 4 | Aftermath |

### Fortify

- **Effect:** Reduce next incoming damage by 50%, store blocked amount as CE
- **Phase:** Clash (resolves first due to high speed priority; shield is active before damage lands)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Fortify also grants adjacent allies 15% damage reduction for 1 round |
| Corrupted | Damage reduction increased to 60%, but CE conversion efficiency reduced to 80% of blocked amount |
| Unbound | Fortify persists for 2 incoming damage instances at 50%/30% reduction |

### Reflect

- **Effect:** Deal damage equal to stored CE, reset CE to 0
- **Phase:** Clash (standard resolution)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Reflect retains 20% of CE after use (does not fully reset) |
| Corrupted | Reflect deals 120% of stored CE but also costs Integrity equal to 10% of CE spent |
| Unbound | Reflect can be split across 2 targets (player chooses CE distribution) |

### Endure

- **Effect:** Absorb ALL damage this phase, add 100% to CE (but you take full damage to Integrity)
- **Phase:** Clash (resolves first; active for the entire Clash phase)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | 30% of damage absorbed by Endure is negated (not taken to Integrity) |
| Corrupted | CE generation increased to 130% of damage absorbed, but stability cost doubled |
| Unbound | Endure can be cancelled mid-phase (after absorbing at least 1 hit) to act in Aftermath |

### Sanctuary

- **Effect:** Heal Integrity equal to 30% of stored CE (does not consume CE)
- **Phase:** Aftermath (resolves after all Clash damage is tallied)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Healing increased to 40% of stored CE and also cleanses 1 debuff |
| Corrupted | Sanctuary converts healing to a damage shield instead (absorbs damage equal to heal amount) |
| Unbound | Sanctuary can be used on an ally instead of self (transfers 30% CE value as healing) |

---

## Synthesis

> *"Something new exists where there was nothing. The pieces remember their shape."*

**Systemic Rule:** Your actions create persistent constructs -- zones, wards, or autonomous effects that operate independently for 2-3 rounds. Your power scales with active construct count. More constructs = more options.

| Action | Base Power | Speed Priority Modifier | Stability Cost | Phase |
|--------|-----------|------------------------|----------------|-------|
| Construct | 100 | -1 | 4 | Clash |
| Enhance | 0 | +1 | 3 | Clash |
| Merge | 0 | +0 | 5 | Aftermath |
| Catalyze | 140 | +2 | 7 | Clash |

### Construct

- **Effect:** Create a persistent zone (damage, buff, or debuff) lasting 3 rounds; construct type determined by player's most-used domain this match
- **Phase:** Clash (resolves late; construct activates on the following round)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Constructs have +1 round duration (4 rounds total) |
| Corrupted | Constructs deal 20% more damage/effect but lose 1 round duration (2 rounds total) |
| Unbound | Player chooses construct type (damage, buff, or debuff) instead of automatic assignment |

### Enhance

- **Effect:** Increase one active construct's potency by 50%
- **Phase:** Clash (resolves early to boost construct before it triggers this round)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Enhance also extends the construct's duration by 1 round |
| Corrupted | Potency increase is 75% but the construct loses 1 round of duration |
| Unbound | Enhance can target an opponent's construct, reducing its potency by 50% instead |

### Merge

- **Effect:** Combine two active constructs into one stronger construct with both effects (merged construct has the longer remaining duration)
- **Phase:** Aftermath (resolves after Clash to allow constructs to trigger before merging)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | Merged construct gains +25% potency on both inherited effects |
| Corrupted | Merged construct gains +40% potency but becomes unstable (+10 instability to Synthesis gem) |
| Unbound | Merge can incorporate an opponent's construct (stealing it and adding its effect to yours) |

### Catalyze

- **Effect:** Trigger all active constructs simultaneously for burst effect (each construct fires at 100% potency), then they expire
- **Phase:** Clash (resolves early to trigger constructs before they might be destroyed by opponent actions)

| Alignment | Variant Effect |
|-----------|----------------|
| Radiant | One construct survives the Catalyze at 50% potency and 1 round remaining |
| Corrupted | Catalyze burst potency is 130% per construct, but costs +3 stability per construct triggered |
| Unbound | Catalyze can be partial: player chooses which constructs to trigger, others remain |

---

## Domain Action Comparison Summary

### By Base Power (Mastery 0, Rare Rarity)

| Rank | Action | Domain | Base Power | Stability Cost | Primary Role |
|------|--------|--------|-----------|----------------|-------------|
| 1 | Surge | Tempest | 180 | 6 | Burst DPS |
| 2 | Momentum Burst | Kinetics | 160 | 5 | Scaling DPS |
| 3 | Reflect | Aegis | 150 | 4 | Reactive DPS |
| 4 | Catalyze | Synthesis | 140 | 7 | Burst DPS |
| 5 | Lightning Chain | Tempest | 140 | 4 | Sustained DPS |
| 6 | Shift Strike | Flux | 135 | 3 | Adaptive DPS |
| 7 | Force Strike | Kinetics | 130 | 3 | Displacement + DPS |
| 8 | Harmonic Strike | Resonance | 125 | 3 | Echo DPS |
| 9 | Corrode | Entropy | 120 | 3 | Debuff DPS |
| 10 | Storm Strike | Tempest | 120 | 3 | Chain Builder |
| 11 | Consume | Void | 115 | 4 | Buff Theft DPS |
| 12 | Decay Chain | Entropy | 110 | 5 | DoT Pressure |
| 13 | Adaptive Shield | Flux | 110 | 4 | Reactive Defense |
| 14 | Dampening Wave | Resonance | 100 | 5 | Disruption |
| 15 | Deflection Field | Kinetics | 100 | 4 | Positioning Defense |
| 16 | Construct | Synthesis | 100 | 4 | Zone Creation |
| 17 | Gravity Well | Kinetics | 90 | 4 | Control |
| 18 | Siphon | Void | 90 | 5 | Resource Theft |
| 19 | Destabilize | Entropy | 80 | 6 | Gem Disruption |
| 20 | Unravel | Entropy | 70 | 4 | Buff Strip |

**Note:** Actions with 0 base power (Amplify, Frequency Shift, Metamorphosis, Phase Walk, Overclock, Fortify, Endure, Sanctuary, Enhance, Merge, Absence, Null Zone) are utility/defensive actions whose value is not measured in direct damage.

---

## Related Pages

- [[Gem System]] -- System overview and design philosophy
- [[Gem System Math]] -- Damage formulas, scaling, DPS modeling
- [[Modifier Gems Reference]] -- Full modifier gem list with compatibility
- [[Gem Identity System]] -- Gem domains, stats, and archetype naming
