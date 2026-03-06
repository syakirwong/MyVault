---
tags:
  - gdd
  - combat
  - gems
  - math
  - balance
---

# Gem System Math

This document contains the mathematical foundations for all gem-related combat calculations. Every formula here is authoritative. If the code disagrees with this document, the code is wrong.

---

## Damage Formula

### Base Damage

```
Base_Damage = Skill_Gem_Power * (1 + Stat_Scaling)
```

Where:

- **Skill_Gem_Power**: Base value defined per gem, modified by mastery level
- **Stat_Scaling**: Derived from the primary stat associated with the skill gem's element

### Stat Scaling (Diminishing Returns)

```
Stat_Scaling = min(Primary_Stat / 100, 2.0) + max(0, (Primary_Stat - 200) / 500)
```

| Primary Stat | Stat_Scaling | Effective Multiplier (1 + Scaling) |
|--------------|--------------|-------------------------------------|
| 50           | 0.50         | 1.50                                |
| 100          | 1.00         | 2.00                                |
| 150          | 1.50         | 2.50                                |
| 200          | 2.00         | 3.00                                |
| 300          | 2.20         | 3.20                                |
| 400          | 2.40         | 3.40                                |
| 500          | 2.60         | 3.60                                |

The soft cap at 200 Primary Stat makes stacking a single stat progressively less efficient. Beyond 200, each additional 100 points yields only +0.20 scaling instead of +1.00. This pushes players toward diversified stat investment.

### Skill Gem Power Ranges by Rarity

| Rarity    | Min Base Power | Max Base Power |
|-----------|----------------|----------------|
| Common    | 50             | 200            |
| Uncommon  | 65             | 230            |
| Rare      | 80             | 260            |
| Epic      | 95             | 280            |
| Legendary | 110            | 300            |

---

## Modifier Application

```
Modified_Damage = Base_Damage * Product(1 + Mod_i) * Alignment_Factor * Crit_Factor
```

Where:

- **Mod_i**: Each modifier gem's bonus value (range: 0.05 to 0.25 per modifier)
- **Alignment_Factor**: Determined by player alignment (see below)
- **Crit_Factor**: 1.0 on normal hit, or (1.0 + Crit_Damage_Bonus) on critical hit

### Alignment Factor Table

| Alignment | Damage Factor | Healing/Shield Factor | Special                                         |
|-----------|---------------|------------------------|--------------------------------------------------|
| Radiant   | 0.85          | 1.30                   | None                                             |
| Corrupted | 1.20          | 0.75                   | Self_Damage = 0.08 * Modified_Damage per cast    |
| Unbound   | 1.00          | 1.00                   | Cooldown_Reduction = 0.20 (20% reduced cooldowns)|

### Modifier Value Ranges

| Modifier Category | Min Bonus | Max Bonus | Typical |
|-------------------|-----------|-----------|---------|
| Damage            | +0.05     | +0.25     | +0.12   |
| Defensive         | +0.05     | +0.20     | +0.10   |
| Utility           | +0.05     | +0.15     | +0.10   |
| Elemental         | +0.05     | +0.20     | +0.12   |
| Synergy           | +0.05     | +0.25     | +0.15   |

---

## Crit Calculation

### Crit Chance

```
Crit_Chance = Base_Crit + DEX / 400 + Gem_Crit_Bonus
```

- **Base_Crit**: 5% for all players
- **DEX / 400**: At 200 DEX = 50% contribution from stat alone
- **Gem_Crit_Bonus**: From modifier gems and skill gem effects
- **Soft Cap**: 50% (diminishing returns beyond this point)
- **Hard Cap**: 75% (cannot exceed regardless of investment)

Diminishing returns formula beyond soft cap:

```
Effective_Crit = 50% + (Raw_Crit - 50%) * 0.5
```

Example: 70% raw crit = 50% + (20% * 0.5) = 60% effective crit.

### Crit Damage

```
Crit_Damage_Multiplier = 1.5 + Sum(Crit_Damage_Modifiers)
```

- Base crit damage: 1.5x (50% bonus damage)
- Cap: 2.5x (100% bonus damage beyond base)
- No diminishing returns on crit damage -- it is gated by modifier slot opportunity cost

---

## DPS Modeling -- Example Builds at Level 50

Assumptions:
- Level 50, ~180 Primary Stat, ~120 Secondary Stat
- All gems at Mastery 5
- Rare-tier gems (3 modifier slots each)
- Standard rotation executed cleanly

| Build          | Domains                      | Gems                                                                  | Alignment | DPS (ST) | DPS (AoE) | Sustain   | Notes                               |
|----------------|------------------------------|-----------------------------------------------------------------------|-----------|----------|-----------|-----------|--------------------------------------|
| Stormkinetic   | Tempest + Kinetics + Entropy + Flux | Storm Strike + Lightning Chain + Force Strike + Corrode         | Corrupted | ~1800    | ~1200     | Low       | Glass cannon, highest burst          |
| Resonant Bastion | Aegis + Resonance + Synthesis + Void | Fortify + Sanctuary + Harmonic Strike + Construct           | Radiant   | ~900     | ~600 + 800 HPS | Very High | Group anchor, lowest solo speed |
| Chaosweaver    | Flux + Entropy + Tempest + Kinetics | Shift Strike + Decay Chain + Storm Strike + Momentum Burst    | Unbound   | ~1400    | ~1000     | Medium    | Best sustained DPS, positioning dep. |

### Stormkinetic Breakdown

```
Lightning Chain (primary damage source):
  Skill_Gem_Power = 185 (Rare, Mastery 5)
  Primary_Stat = 180 -> Stat_Scaling = 1.80
  Base_Damage = 185 * (1 + 1.80) = 185 * 2.80 = 518 per action
  Modifiers: Sharpened Edge (+12%), Overload (+20%), Combo Finisher (+35%)
  Product(1 + Mod_i) = 1.12 * 1.20 * 1.35 = 1.8144
  Alignment (Corrupted): 1.20
  Non-crit: 518 * 1.8144 * 1.20 = 1127.68
  Crit (50% chance, 2.0x): 518 * 1.8144 * 1.20 * 2.0 = 2255.36
  Expected per action: 0.50 * 1127.68 + 0.50 * 2255.36 = 1691.52
  Self-damage per cast: 0.08 * 1691.52 = 135.32

  Storm Strike builds Chain stacks; Lightning Chain + Corrode layered per round
  Full 4-action round with Chain scaling: ~5400 damage
  Effective DPS: ~1800
```

### Resonant Bastion Breakdown

```
Harmonic Strike (primary damage source):
  Skill_Gem_Power = 165 (Rare, Mastery 5)
  Primary_Stat = 170 -> Stat_Scaling = 1.70
  Base_Damage = 165 * (1 + 1.70) = 165 * 2.70 = 445.5
  Modifiers: Elemental Focus (+18%), Duration Extend (+40% on constructs, no damage)
  Alignment (Radiant): 0.85
  Non-crit: 445.5 * 1.18 * 0.85 = 446.73
  DPS contribution from Harmonic Strike: ~450 DPS

Sanctuary (Aegis):
  Heals Integrity equal to 30% of stored CE per round
  With Fortify generating CE: ~500 Integrity healed per round cycle
  Construct passive sustain: ~300 additional HPS via zone effects
  Total Sustain: ~800 HPS
```

### Chaosweaver Breakdown

```
Shift Strike (primary):
  Skill_Gem_Power = 179 (Rare, Mastery 5)
  Primary_Stat = 185 -> Stat_Scaling = 1.85
  Base_Damage = 179 * 2.85 = 510.15
  Modifiers: Sharpened Edge (+12%), Penetration (15% armor ignore), Quicksilver (-15% stability cost)
  Alignment (Unbound): 1.00
  Non-crit: 510.15 * 1.12 * 1.00 = 571.37
  Effective with Decay Chain DoT layering: ~700 effective DPS from Shift Strike rotation

Decay Chain: Sustained DoT pressure amplified by Entropy systemic rule (+50% debuff duration)
  Effective full-build DPS: ~1400 sustained (higher with Momentum Burst scaling)

Full rotation DPS: ~1400 sustained
```

---

## Balance Targets

| Metric                          | Target               | Acceptable Variance |
|---------------------------------|----------------------|---------------------|
| Solo dungeon clear time         | 15-25 minutes        | +/- 30%             |
| Group dungeon clear time        | 10-18 minutes        | +/- 20%             |
| PvP 1v1 win rate vs field       | 40-60%               | No build above 60%  |
| PvP group: composition impact   | High                 | No single-build req |
| Time to viable endgame build    | 20-30 hours          | --                  |
| Time to optimized endgame build | 80-120 hours         | --                  |

### Balance Philosophy

- **Solo dungeons**: All builds clear within 30% of each other. A 15-minute DPS clear and a 25-minute tank clear are both acceptable. Beyond 30% variance indicates a balance failure.
- **Group content**: No specific build is required. Optimal groups have role diversity, but 4 DPS can clear (slower). 4 healers cannot clear (correctly).
- **PvP 1v1**: Rock-paper-scissors balance. Burst beats sustain. Sustain beats control. Control beats burst. No build exceeds 60% win rate against the full field.
- **PvP group**: Composition matters more than individual power. A coordinated 3-stack of mid-tier builds beats 3 uncoordinated optimal builds.

---

## Modifier Stacking Rules

### Within Same Category (Additive with Diminishing Returns)

| Same-Type Modifier Count | Value Applied |
|--------------------------|---------------|
| 1st modifier             | 100% value    |
| 2nd modifier (same type) | 75% value     |
| 3rd modifier (same type) | 50% value     |

Example: Three +12% damage modifiers:

```
Total = 12% + (12% * 0.75) + (12% * 0.50)
     = 12% + 9% + 6%
     = 27% (not 36%)
```

### Across Categories (Multiplicative, No Diminishing Returns)

```
Final_Multiplier = (1 + Damage_Mods) * (1 + Crit_Mods) * (1 + Element_Mods) * Alignment_Factor
```

Cross-category stacking always applies at full value. This incentivizes diversified modifier selection over stacking a single category.

### Stacking Example: Diversified vs Stacked

**Stacked (3 damage mods at +12%):**

```
Damage category total: 1 + 0.12 + 0.09 + 0.06 = 1.27
No other categories.
Final multiplier: 1.27
```

**Diversified (1 damage +12%, 1 crit +15%, 1 element +18%):**

```
Damage: 1.12
Crit: 1.15
Element: 1.18
Final multiplier: 1.12 * 1.15 * 1.18 = 1.519
```

Diversified wins by 19.6%. This is intentional.

---

## Power Budget Per Build

Each build has a total power budget of approximately 100 points distributed across five axes:

| Axis          | Min | Max | Description                        |
|---------------|-----|-----|------------------------------------|
| Damage        | 0   | 40  | Raw output, burst and sustained    |
| Survivability | 0   | 30  | HP, shields, damage reduction      |
| Utility       | 0   | 20  | CC, debuffs, vision, interrupts    |
| Mobility      | 0   | 15  | Movement speed, dashes, teleports  |
| Support       | 0   | 25  | Healing, buffing, resource gen     |

**Target total: 80-100 points regardless of build.** No build should exceed 100. No viable build should fall below 80.

### Example Power Budgets

| Build            | Damage | Survivability | Utility | Mobility | Support | Total |
|------------------|--------|---------------|---------|----------|---------|-------|
| Stormkinetic     | 38     | 10            | 12      | 15       | 5       | 80    |
| Resonant Bastion | 15     | 28            | 15      | 5        | 25      | 88    |
| Chaosweaver      | 32     | 15            | 18      | 15       | 10      | 90    |

This ensures no build dominates all axes. A Stormkinetic deals maximum damage but has the lowest survivability and support. A Resonant Bastion sustains a group but clears solo content slowly. Trade-offs are enforced by the budget, not by player choice alone.

---

## Gem Scaling Table

| Gem Tier  | Base Power | Power at Mastery 5 | Power at Mastery 10 |
|-----------|------------|---------------------|----------------------|
| Common    | 100        | 120                 | 145                  |
| Uncommon  | 120        | 145                 | 175                  |
| Rare      | 145        | 175                 | 215                  |
| Epic      | 170        | 210                 | 260                  |
| Legendary | 200        | 250                 | 310                  |

### Mastery Scaling Formula

```
Power_at_Mastery = Base_Power * (1 + 0.04 * M + 0.005 * M^2)
```

Where M = Mastery Level (0-10).

### Verification Table

| Mastery Level | Multiplier Calculation         | Multiplier | Common (100) | Legendary (200) |
|---------------|--------------------------------|------------|--------------|------------------|
| 0             | 1 + 0 + 0                     | 1.000      | 100          | 200              |
| 1             | 1 + 0.04 + 0.005              | 1.045      | 104.5        | 209              |
| 2             | 1 + 0.08 + 0.020              | 1.100      | 110          | 220              |
| 3             | 1 + 0.12 + 0.045              | 1.165      | 116.5        | 233              |
| 4             | 1 + 0.16 + 0.080              | 1.240      | 124          | 248              |
| 5             | 1 + 0.20 + 0.125              | 1.325      | 132.5        | 265              |
| 6             | 1 + 0.24 + 0.180              | 1.420      | 142          | 284              |
| 7             | 1 + 0.28 + 0.245              | 1.525      | 152.5        | 305              |
| 8             | 1 + 0.32 + 0.320              | 1.640      | 164          | 328              |
| 9             | 1 + 0.36 + 0.405              | 1.765      | 176.5        | 353              |
| 10            | 1 + 0.40 + 0.500              | 1.900      | 190          | 380              |

**Note on rounding**: The summary table at top uses rounded values tuned for feel (e.g., Common Mastery 5 = 120 instead of 132.5). Final game values will be rounded to nearest 5 for readability. The formula is the source of truth; the table values are presentation targets that may be adjusted within +/- 5%.

---

## Combinatorial Space

```
Combinations = Origins * Gem_Loadouts * Modifier_Permutations * Alignments
```

### Current MVP Numbers

| Variable               | Count  |
|------------------------|--------|
| Origins                | 6      |
| Gem domains            | 8      |
| Gem slots per player   | 4      |
| Modifier gems (total)  | 25     |
| Modifier slots per gem | 2-3    |
| Alignments             | 3      |

### Rough Combinatorial Estimate

```
Gem loadouts per player: C(8,4) = 70 (choose 4 domains from 8)
Modifier permutations per skill (3 slots, 25 mods, compatibility filter ~40%):
  Effective pool per slot: ~10 compatible mods
  Permutations per skill: C(10,3) = 120
Modifier permutations per build: 120^4 = 207,360,000
Across loadouts, Origins, and alignments: 70 * 6 * 207,360,000 * 3 = 261,273,600,000
```

**Theoretical combinations: ~261 billion.**

Most are non-viable. Filtering for synergy and competitive viability:

- Estimated viable competitive builds at any given meta: **50-100+**
- This is the game's competitive moat. No two seasons should have the same meta.

---

## Related Pages

- [[Gem System]] -- System overview and design philosophy
- [[Skill Gems Reference]] -- Full skill gem specifications
- [[Modifier Gems Reference]] -- Full modifier gem specifications
- [[Gem Identity System]] -- Gem domains, stats, and archetype naming
