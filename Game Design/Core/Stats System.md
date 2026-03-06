---
tags:
  - gdd
  - core
  - stats
  - balance
---

# Stats System

## Design Philosophy

Stats are the **quiet foundation**, not the loud ceiling. In Stormbound Online, stats don't make you powerful — gems and builds do. Stats create the baseline from which gem actions scale, and they ensure that every character has meaningful trade-offs baked into their loadout choices.

No stat should be dumpable. Every stat should matter for every build, even if some matter more.

---

## The Six Stats

### Force

> *Raw power. The distance between you and your target shrinks.*

| Aspect | Effect |
|---|---|
| Primary | Damage scaling for physical/kinetic actions |
| Secondary | Displacement distance (Kinetics push/pull) |
| Scaling | See [[Gem System Math]]: `Stat_Scaling = min(Force / 100, 2.0) + max(0, (Force - 200) / 500)` |
| Soft Cap | 200 (after which diminishing returns apply) |
| Primary Domains | Kinetics, Tempest |

### Precision

> *The edge between hit and miss. Between first and second.*

| Aspect | Effect |
|---|---|
| Primary | Crit chance: `Base_Crit(5%) + Precision / 400` |
| Secondary | Speed priority in Clash phase |
| Tertiary | Action accuracy (reduces whiff chance in unstable gem states) |
| Soft Cap | 200 Precision = 50% crit contribution (hard cap 75% effective crit) |
| Primary Domains | Tempest, Flux |

### Resilience

> *What doesn't kill you is running out of time.*

| Aspect | Effect |
|---|---|
| Primary | Integrity pool: `Base(100) + Resilience * 2` |
| Secondary | Damage reduction: `min(Resilience / 500, 0.20)` — caps at 20% flat reduction |
| Tertiary | Gem stability buffer: gems lose 1 less stability per hit when Resilience > 150 |
| Soft Cap | 200 Resilience = 400 Integrity, 20% DR approaches |
| Primary Domains | Aegis, Synthesis |

### Acuity

> *You see what others miss. The fraying edges, the hidden cost.*

| Aspect | Effect |
|---|---|
| Primary | Debuff potency: debuff duration extended by `Acuity / 200` rounds (rounded) |
| Secondary | Resource efficiency: action resource costs reduced by `min(Acuity / 400, 0.25)` |
| Tertiary | Combat log detail: at Acuity 100+, log reveals enemy gem stability estimates; at 200+, reveals mutation state |
| Soft Cap | 200 Acuity = +1 round debuff duration, 25% resource reduction approaches |
| Primary Domains | Entropy, Void |

### Attunement

> *The first note. The echo. The resonance between them.*

| Aspect | Effect |
|---|---|
| Primary | Echo power: Resonance domain echoes deal `40% + Attunement / 500` base power |
| Secondary | Construct duration: Synthesis constructs last `+floor(Attunement / 100)` additional rounds |
| Tertiary | Resonance trigger chance: `Base_Chance + Attunement / 400` bonus |
| Soft Cap | 200 Attunement = 80% echo power, +2 construct rounds, +50% resonance trigger bonus |
| Primary Domains | Resonance, Synthesis |

### Instinct

> *You moved before you knew why. The draw answered before you asked.*

| Aspect | Effect |
|---|---|
| Primary | Draw pool bonus: at Instinct 100+, draw 1 additional action per round |
| Secondary | Conditional action availability: conditional Slot 3 unlocks at Instinct 50+ (instead of requiring build mastery) |
| Tertiary | Whiff recovery: failed actions at Instinct 150+ refund 50% of their stability cost |
| Soft Cap | 200 Instinct — draw bonus doesn't stack further, but whiff recovery reaches 75% |
| Primary Domains | Flux, Kinetics |

---

## Stat Sources

Stats come from four sources. The total stat profile is the sum of all sources.

### 1. Character Level (Universal)

Every level grants a flat bonus to ALL stats.

```
Level_Stat_Bonus = 2 * Level (per stat)
```

| Level | Bonus Per Stat | Total Stats from Level |
|---|---|---|
| 1 | +2 | +12 (across 6 stats) |
| 25 | +50 | +300 |
| 50 | +100 | +600 |

At level 50, every character has +100 in every stat from levelling alone. This is the universal baseline.

### 2. Origin (Fixed Bias)

Selected at character creation. See [[Experience & Levelling]] for full Origin table.

| Origin | Primary Stat | Secondary Stat |
|---|---|---|
| Stormborn | Force +15 | Precision +10 |
| Voidtouched | Acuity +15 | Attunement +10 |
| Ironsworn | Resilience +15 | Force +10 |
| Wavereader | Attunement +15 | Acuity +10 |
| Drifter | Instinct +15 | Precision +10 |
| Shaper | Precision +15 | Attunement +10 |

Origin bias is flat and does not scale. It nudges starting builds without constraining endgame choices.

### 3. Equipped Gems (Dynamic)

Each gem domain provides stats weighted toward its primary domains. This is the **primary source of stat differentiation** between builds.

#### Stat Contribution Per Gem Domain

| Domain | Force | Precision | Resilience | Acuity | Attunement | Instinct | Total |
|---|---|---|---|---|---|---|---|
| Kinetics | 12 | 4 | 4 | 2 | 2 | 8 | 32 |
| Entropy | 4 | 4 | 2 | 12 | 4 | 6 | 32 |
| Resonance | 2 | 4 | 4 | 6 | 12 | 4 | 32 |
| Void | 4 | 2 | 4 | 12 | 6 | 4 | 32 |
| Flux | 4 | 8 | 2 | 4 | 4 | 12 | 34 |
| Tempest | 10 | 10 | 2 | 2 | 2 | 6 | 32 |
| Aegis | 4 | 2 | 14 | 2 | 4 | 6 | 32 |
| Synthesis | 2 | 4 | 10 | 4 | 10 | 2 | 32 |

**Notes:**
- Each gem contributes ~32 stat points (Flux is slightly higher due to its transformative nature requiring broader stat access)
- 4 equipped gems = ~128 stat points distributed across your build's stat profile
- Gem stat contribution scales with gem mastery: `Gem_Stats * (1 + 0.02 * Mastery_Level)`
- At Mastery 10: gem stats are 1.20x base (a meaningful but not dominant bonus)

#### Example Build Stat Profiles (4 gems, Mastery 5, Level 50)

**Aggressive Build: Kinetics + Tempest + Entropy + Void**

| Stat | Level | Origin (Stormborn) | Gems (base) | Gems (M5 scaled) | Total |
|---|---|---|---|---|---|
| Force | 100 | +15 | 30 | 33 | 148 |
| Precision | 100 | +10 | 20 | 22 | 132 |
| Resilience | 100 | — | 12 | 13 | 113 |
| Acuity | 100 | — | 30 | 33 | 133 |
| Attunement | 100 | — | 14 | 15 | 115 |
| Instinct | 100 | — | 24 | 26 | 126 |

High Force and Acuity. Low Resilience and Attunement. Glass cannon profile — exactly what this loadout should produce.

**Defensive Build: Aegis + Synthesis + Resonance + Kinetics**

| Stat | Level | Origin (Ironsworn) | Gems (base) | Gems (M5 scaled) | Total |
|---|---|---|---|---|---|
| Force | 100 | +10 | 20 | 22 | 132 |
| Precision | 100 | — | 14 | 15 | 115 |
| Resilience | 100 | +15 | 32 | 35 | 150 |
| Acuity | 100 | — | 14 | 15 | 115 |
| Attunement | 100 | — | 28 | 31 | 131 |
| Instinct | 100 | — | 20 | 22 | 122 |

High Resilience and Attunement. Moderate Force. Built to endure and outlast.

### 4. Gear (Base Stats Only)

Gear provides flat stat bonuses. Per [[Progression Systems]], gear provides BASE STATS ONLY — no skill power, no gem interaction.

| Gear Slot | Stat Budget |
|---|---|
| Weapon | 10-25 stats (split across 2 stats) |
| Armor | 10-25 stats (split across 2 stats) |
| Accessory 1 | 5-15 stats (single stat focus) |
| Accessory 2 | 5-15 stats (single stat focus) |

| Gear Rarity | Stat Budget per Slot |
|---|---|
| Common | 10-12 |
| Uncommon | 13-16 |
| Rare | 17-20 |
| Masterwork | 21-23 |
| Legendary | 24-25 |

**Total gear contribution at Legendary tier:** ~80-90 stat points across 4 slots.

---

## Stat Budget Summary (Level 50)

| Source | Stat Points (approx.) | % of Total |
|---|---|---|
| Character Level | 600 (100 per stat) | 62% |
| Equipped Gems | 128-136 (distributed) | 14% |
| Gem Mastery (at M5) | ~13 (10% gem bonus) | 1% |
| Origin | 25 (fixed) | 3% |
| Gear (Rare tier) | ~70 | 7% |
| Gear (Legendary) | ~85 | 9% |
| Prestige (P60, max) | 5% of total | 4% |

**Design intent:** Character level is the majority contributor, ensuring all level 50 characters are on roughly equal footing. Gems and gear provide the differentiation layer. No single source dominates enough to create insurmountable gaps.

---

## Stat Interactions with Combat

| Combat Mechanic | Primary Stat | Secondary Stat | Formula Reference |
|---|---|---|---|
| Damage dealt | Force | Precision (crit) | [[Gem System Math]] — Base Damage formula |
| Crit chance | Precision | — | `5% + Precision / 400` |
| Speed priority | Precision | Instinct | Precision comparison + Instinct tiebreaker |
| Integrity pool | Resilience | — | `100 + Resilience * 2` |
| Damage reduction | Resilience | — | `min(Resilience / 500, 0.20)` |
| Debuff duration | Acuity | — | `+floor(Acuity / 200)` rounds |
| Resource cost | Acuity | — | `-min(Acuity / 400, 0.25)` |
| Echo power | Attunement | — | `40% + Attunement / 500` |
| Construct duration | Attunement | — | `+floor(Attunement / 100)` rounds |
| Resonance trigger | Attunement | — | `Base + Attunement / 400` |
| Draw pool | Instinct | — | +1 at 100+, no additional past 200 |
| Whiff recovery | Instinct | — | 50% at 150, 75% at 200 |

---

## Stat Soft Caps and Diminishing Returns

All stats follow the same diminishing returns curve defined in [[Gem System Math]]:

```
Effective_Value = min(Stat / 100, 2.0) + max(0, (Stat - 200) / 500)
```

| Stat Value | Effective Scaling | Marginal Return |
|---|---|---|
| 0-100 | Linear (1:1) | Full value |
| 101-200 | Linear (1:1) | Full value |
| 201-300 | 0.20 per 100 | 20% efficiency |
| 301-400 | 0.20 per 100 | 20% efficiency |
| 400+ | 0.20 per 100 | 20% efficiency (hard diminishing) |

**Practical ceiling:** Most builds land between 110-160 in their primary stats and 100-125 in secondaries. Pushing a stat beyond 200 requires extreme specialization (all 4 gems + gear + origin aligned) and yields minimal returns. The system rewards breadth over depth.

---

## No Free Stat Points

There are **no allocatable stat points**. Stats are fully determined by:
- Your level (everyone gets the same)
- Your origin (chosen once)
- Your gems (chosen dynamically)
- Your gear (acquired through play)

This is deliberate. Stat allocation creates illusions of choice that resolve to cookie-cutter builds on wikis. In Stormbound, your stats ARE your build — change your gems, change your stats. The feedback loop is immediate and visible.

---

**Related:** [[Experience & Levelling]] | [[Gem Identity System]] | [[Gem System Math]] | [[Progression Systems]] | [[Combat System]] | [[Factions System]]