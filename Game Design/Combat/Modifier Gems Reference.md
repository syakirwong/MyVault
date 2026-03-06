---
tags:
  - gdd
  - combat
  - gems
  - reference
---

# Modifier Gems Reference

This document defines all 25 modifier gems for the MVP release. Modifier gems do not function independently -- they must be slotted into a compatible skill gem to take effect.

For the system overview, see [[Gem System]].
For damage formulas and stacking rules, see [[Gem System Math]].
For skill gems these modifiers attach to, see [[Skill Gems Reference]].

---

## How to Read This Reference

- **Effect**: What the modifier does when slotted into a compatible skill gem.
- **Value**: The magnitude of the effect. Subject to diminishing returns if stacking same-category modifiers (see Stacking Rules below).
- **Compatible Elements**: The skill gem must have one of these elements for the modifier to be slotted.
- **Rarity**: Determines drop rate and acquisition difficulty. See [[Gem System]] for rarity drop rates.

---

## Damage Modifiers (7)

Damage modifiers increase raw output. Stacking multiple damage modifiers on the same skill gem triggers diminishing returns within this category.

| #  | Gem             | Effect                              | Value                    | Compatible Elements     | Rarity   |
|----|-----------------|--------------------------------------|--------------------------|-------------------------|----------|
| 1  | Sharpened Edge  | +% base damage                      | +12%                     | Physical, Wind          | Common   |
| 2  | Overload        | +% damage, +% self-damage           | +20% dmg, 5% self-dmg   | Storm, Fire             | Uncommon |
| 3  | Penetration     | Ignore % of target armor            | 15% armor ignore         | Physical, Void          | Rare     |
| 4  | Elemental Focus | +% elemental damage                 | +18%                     | Fire, Storm, Wind, Nature | Common |
| 5  | Berserker       | +% damage below 50% HP              | +30%                     | Physical, Fire          | Uncommon |
| 6  | Assassin's Mark | +% damage from behind               | +25%                     | Void, Physical          | Rare     |
| 7  | Celestial Fury  | +% damage to alignment-opposed      | +22%                     | Light, Void             | Epic     |

### Damage Modifier Notes

- **Overload** self-damage is calculated on the modified damage value, not base. At high investment, this can be lethal without sustain. Corrupted alignment compounds this risk (0.08 * Modified_Damage from alignment + 0.05 from Overload = 13% self-damage per cast).
- **Berserker** is conditionally powerful. Below 50% HP it is the strongest single damage modifier, but building around it requires either very high sustain or acceptance of death risk.
- **Celestial Fury** requires knowledge of enemy alignment. In PvP, this is always known. In PvE, Corrupted enemies are marked visually.
- **Penetration** is multiplicative with damage in practice because armor reduces incoming damage by a percentage. Ignoring 15% of armor on a target with 40% damage reduction effectively yields ~6% more damage dealt (varies by target armor value).

---

## Defensive Modifiers (5)

Defensive modifiers add survivability to offensive skills. They never trigger diminishing returns with damage modifiers (cross-category stacking is always full value).

| #  | Gem          | Effect                                  | Value                    | Compatible Elements | Rarity   |
|----|--------------|------------------------------------------|--------------------------|---------------------|----------|
| 8  | Iron Skin    | +% damage reduction during skill cast   | 15% DR                   | Physical, Earth     | Common   |
| 9  | Lifetap      | Heal % of damage dealt by this skill    | 8% lifesteal             | Nature, Void        | Uncommon |
| 10 | Barrier      | Skill generates a shield on use         | 10% max HP shield (3s)   | Light, Wind         | Rare     |
| 11 | Retaliation  | Reflect % damage when hit during skill  | 20% reflect              | Fire, Storm         | Uncommon |
| 12 | Endurance    | +% HP regen for 4s after skill use      | 3% HP/s for 4s           | Nature, Earth       | Common   |

### Defensive Modifier Notes

- **Iron Skin** applies during action resolution in the Clash phase. Best on actions with high stability cost where the damage reduction window protects against counterattacks (e.g., Endure, Surge).
- **Lifetap** at 8% sustains approximately 80-160 HP per hit at mid-game damage values. Significant over a full rotation but not enough to replace dedicated healing.
- **Barrier** shield value scales with max HP, making it better on tankier builds. The 3s duration means shields from rapid casts do not stack -- new shield replaces old.
- **Endurance** total healing: 3% HP/s for 4s = 12% max HP recovered per skill use. On a 4-skill rotation, this can sustain approximately 48% max HP per rotation cycle.

---

## Utility Modifiers (5)

Utility modifiers alter skill targeting, cooldowns, or resource management. They provide strategic flexibility rather than raw power.

| #  | Gem             | Effect                                  | Value                      | Compatible Elements | Rarity   |
|----|-----------------|------------------------------------------|----------------------------|---------------------|----------|
| 13 | Quicksilver     | -% cooldown on this skill              | -15% CD                    | Wind, Storm         | Common   |
| 14 | Chain Reaction  | Skill bounces to nearby enemy          | 1 bounce, 60% dmg          | Storm, Fire         | Rare     |
| 15 | AoE Conversion  | Single-target becomes AoE              | 3m radius, 70% dmg         | Fire, Light         | Epic     |
| 16 | Duration Extend | +% buff/debuff duration                | +40% duration              | Nature, Light       | Common   |
| 17 | Resource Return | Refund % resource cost on kill         | 30% refund                 | Void, Wind          | Uncommon |

### Utility Modifier Notes

- **Quicksilver** reduces stability cost per use by 15%. Stacks with Unbound alignment's stability efficiency bonus. Example: Surge (6 stability cost) becomes 5.1, or ~4 with Unbound variant's bonus.
- **Chain Reaction** bounce range is 6m. The bounce target is the nearest enemy not already hit. At 60% damage, the bounce is meaningful but not a substitute for true AoE.
- **AoE Conversion** is the most build-defining modifier. Converting a single-target action to AoE creates a 3m explosion at each impact point -- fundamentally changing the build's playstyle. The 70% damage penalty prevents it from outperforming dedicated AoE actions.
- **Duration Extend** applies to buffs and debuffs the action creates (e.g., Corrode's 2-round defense reduction becomes ~3 rounds) and persistent effects (constructs, echoes). Does not extend the action's own resolution timing.
- **Resource Return** only triggers on killing blows. Effective in trash clearing, useless against bosses.

---

## Elemental Modifiers (4)

Elemental modifiers add secondary effects tied to specific elements. They introduce new damage types or crowd control.

| #  | Gem            | Effect                           | Value                    | Compatible Elements | Rarity   |
|----|----------------|-----------------------------------|--------------------------|---------------------|----------|
| 18 | Frost Infusion | Add slow effect to skill         | 30% slow, 2s duration   | Water, Wind         | Uncommon |
| 19 | Ignite         | Add burn DoT to skill            | 5% base dmg/s, 3s       | Fire                | Common   |
| 20 | Shock          | Chance to stun on hit            | 15% stun chance, 1s     | Storm               | Uncommon |
| 21 | Void Touch     | Convert 30% damage to Void       | 30% conversion          | Void                | Rare     |

### Elemental Modifier Notes

- **Frost Infusion** slow does not stack with itself. Reapplication refreshes the duration. 30% slow is significant in PvP (reduces both movement and attack speed).
- **Ignite** DoT: 5% of base damage per second for 3 seconds = 15% base damage total. On a 150 base power gem at Mastery 5 (175 effective), Ignite adds ~26.25 damage over 3s. DoT does not crit.
- **Shock** stun is rolled per action that deals damage. On actions that trigger echoes or chain effects (e.g., Harmonic Strike echo, Lightning Chain with stacks), effective stun probability increases per additional hit:
  - 2 hits: `1 - (0.85)^2 = 27.8%` chance of at least one stun
  - 3 hits: `1 - (0.85)^3 = 38.6%` chance of at least one stun
- **Void Touch** is a conversion, not additional damage. It changes 30% of the damage to Void element, which bypasses physical armor but is resisted by void resistance. This is the only way to give non-Void skills access to Void damage.

### Conversion Rule Reminder

Maximum one Conversion modifier per skill gem. Void Touch is currently the only Conversion modifier in the MVP set. This rule exists to prevent element-stacking exploits in future expansions.

---

## Synergy Modifiers (4)

Synergy modifiers reward specific build choices, playstyles, or alignment investment. They have the highest ceiling but require more setup.

| #  | Gem              | Effect                                        | Value                      | Compatible Elements | Rarity    |
|----|------------------|------------------------------------------------|----------------------------|---------------------|-----------|
| 22 | Companion Echo   | Skill triggers a companion attack             | 40% base dmg companion hit | Nature, Light       | Epic      |
| 23 | Alignment Surge  | +% effect per 20 alignment depth              | +5% per 20 depth           | All                 | Rare      |
| 24 | Combo Finisher   | Bonus damage if used within 2s of another skill | +35%                      | Physical, Storm     | Uncommon  |
| 25 | Gem Resonance    | +% power if all equipped gems share element   | +10% per shared gem        | All                 | Legendary |

### Synergy Modifier Notes

- **Companion Echo** requires the player to have a companion active (summoned creature, Synthesis construct, or companion from NPC questline). The companion attack scales with the companion's stats, not the skill gem's. The 40% value is a base -- companion gear and talents modify it further.
- **Alignment Surge** scales with alignment depth (0-100 scale). At maximum alignment depth (100): `5 * (100/20) = 25%` bonus. This is a significant reward for deep alignment commitment but comes at the cost of flexibility.

  | Alignment Depth | Bonus from Alignment Surge |
  |-----------------|----------------------------|
  | 20              | +5%                        |
  | 40              | +10%                       |
  | 60              | +15%                       |
  | 80              | +20%                       |
  | 100             | +25%                       |

- **Combo Finisher** requires casting another skill within 2 seconds before using the modified skill. This rewards fluid rotation play and punishes one-button spam. The 2s window is tight enough to require intention but loose enough to accommodate normal play latency.
- **Gem Resonance** is the strongest modifier in the game when fully online. With 4 equipped gems sharing the same element: `+10% * 4 = +40% power` to all actions. However, this severely restricts build diversity (mono-element builds sacrifice cross-element coverage). Currently only achievable on mono-Tempest and mono-Aegis domain loadouts at MVP.

  | Shared Gems | Gem Resonance Bonus |
  |-------------|---------------------|
  | 1 (just self)| +10%               |
  | 2           | +20%                |
  | 3           | +30%                |
  | 4           | +40%                |

---

## Modifier Stacking Rules

### Same-Category Diminishing Returns

When multiple modifiers from the **same category** are slotted across a player's build, diminishing returns apply:

| Same-Category Count | Value Applied | Example (+12% each)       |
|---------------------|---------------|---------------------------|
| 1st modifier        | 100%          | +12.0%                    |
| 2nd modifier        | 75%           | +9.0%                     |
| 3rd modifier        | 50%           | +6.0%                     |
| **Cumulative**      |               | **+27.0% (not +36.0%)**   |

### Cross-Category Stacking

Modifiers from **different categories** always apply at full value and multiply with each other:

```
Final = Base * (1 + Damage_Mods) * (1 + Defensive_Mods) * (1 + Utility_Mods)
       * (1 + Elemental_Mods) * (1 + Synergy_Mods) * Alignment_Factor
```

### Stacking Comparison: 3 Damage vs 1 of Each

**3 Damage Mods (Sharpened Edge +12%, Overload +20%, Elemental Focus +18%):**

All same category (damage):

```
Total damage bonus = 20% (1st, full) + 13.5% (2nd, 75% of 18%) + 6% (3rd, 50% of 12%)
                   = 39.5%
Final multiplier from mods: 1.395
```

Note: Highest value modifier gets full value, then descending. The system automatically applies the best ordering.

**1 Damage + 1 Defensive + 1 Synergy (Sharpened Edge +12%, Lifetap 8% steal, Combo Finisher +35%):**

All different categories:

```
Damage: 1.12
Defensive: 1.08 (effective as sustain, not direct multiplier on damage)
Synergy: 1.35
Final multiplier on damage: 1.12 * 1.35 = 1.512
Plus 8% lifesteal for sustain
```

Cross-category is multiplicative and yields higher total output plus sustain.

---

## Full Modifier Index (Alphabetical)

| #  | Gem              | Category  | Rarity    | Compatible Elements          |
|----|------------------|-----------|-----------|------------------------------|
| 15 | AoE Conversion   | Utility   | Epic      | Fire, Light                  |
| 23 | Alignment Surge  | Synergy   | Rare      | All                          |
| 6  | Assassin's Mark  | Damage    | Rare      | Void, Physical               |
| 10 | Barrier          | Defensive | Rare      | Light, Wind                  |
| 5  | Berserker        | Damage    | Uncommon  | Physical, Fire               |
| 7  | Celestial Fury   | Damage    | Epic      | Light, Void                  |
| 14 | Chain Reaction   | Utility   | Rare      | Storm, Fire                  |
| 24 | Combo Finisher   | Synergy   | Uncommon  | Physical, Storm              |
| 22 | Companion Echo   | Synergy   | Epic      | Nature, Light                |
| 16 | Duration Extend  | Utility   | Common    | Nature, Light                |
| 4  | Elemental Focus  | Damage    | Common    | Fire, Storm, Wind, Nature    |
| 12 | Endurance        | Defensive | Common    | Nature, Earth                |
| 18 | Frost Infusion   | Elemental | Uncommon  | Water, Wind                  |
| 25 | Gem Resonance    | Synergy   | Legendary | All                          |
| 19 | Ignite           | Elemental | Common    | Fire                         |
| 8  | Iron Skin        | Defensive | Common    | Physical, Earth              |
| 9  | Lifetap          | Defensive | Uncommon  | Nature, Void                 |
| 2  | Overload         | Damage    | Uncommon  | Storm, Fire                  |
| 3  | Penetration      | Damage    | Rare      | Physical, Void               |
| 13 | Quicksilver      | Utility   | Common    | Wind, Storm                  |
| 17 | Resource Return  | Utility   | Uncommon  | Void, Wind                   |
| 11 | Retaliation      | Defensive | Uncommon  | Fire, Storm                  |
| 1  | Sharpened Edge   | Damage    | Common    | Physical, Wind               |
| 20 | Shock            | Elemental | Uncommon  | Storm                        |
| 21 | Void Touch       | Elemental | Rare      | Void                         |

---

## Related Pages

- [[Gem System]] -- System overview and design philosophy
- [[Gem System Math]] -- Damage formulas, stacking math, DPS modeling
- [[Skill Gems Reference]] -- Skill gems these modifiers attach to
