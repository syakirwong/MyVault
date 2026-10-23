---
tags:
  - gdd
  - combat
  - gems
---

> [!warning] Archived 2026-10-23
> Part of the Stormbound **MMO** design, superseded when the game pivoted to a single-player narrative game. See [[Stormbound GDD]]. Kept as source material.

# Gem System

## Overview

Gems are the core customization mechanic. They replace traditional skill trees entirely. Players equip **Skill Gems** (active abilities) and **Modifier Gems** (passive modifiers that alter skill behavior). Every build is defined by the combination of gems slotted, not by a fixed progression path.

The system prioritizes horizontal depth over vertical power. A well-constructed common gem build should remain competitive against a lazily assembled legendary gem build. Rarity gates options, not power floors.

---

## Gem Types

### 1. Skill Gems -- Active Abilities

Skill gems are the abilities a player actually casts. They fall into three functional categories:

- **Offensive** -- Deal damage (single target, AoE, DoT)
- **Defensive** -- Mitigate damage (shields, heals, damage reduction)
- **Utility** -- Provide tactical advantage (crowd control, movement, buffs)

Each skill gem has a base element, a base power value, and alignment variants that change behavior depending on the player's [[Alignment System]] commitment.

### 2. Modifier Gems -- Passive Behavior Alteration

Modifier gems do not function alone. They attach to a skill gem and change how that skill behaves:

- Element conversion (e.g., physical to void)
- Targeting conversion (e.g., single-target to AoE)
- Chain effects (bouncing projectiles)
- Conditional bonuses (bonus damage from behind, below 50% HP)
- Defensive layers (lifesteal on hit, shield on cast)

---

## Gem Slots

- Each player has **4 Skill Gem slots**
- Each Skill Gem has **2-3 Modifier Gem slots** (determined by gem rarity/tier)
- Total modifier capacity per build: **8-12 modifiers**
- **Post-MVP:** 1 Companion Gem slot (see [[#Companion Gem (Post-MVP)]])

### Slot Allocation by Rarity

| Gem Rarity  | Modifier Slots |
|-------------|----------------|
| Common      | 2              |
| Uncommon    | 2              |
| Rare        | 3              |
| Epic        | 3              |
| Legendary   | 3 + 1 unique   |

The "unique" legendary slot accepts only the modifier that is bound to that specific legendary gem. This prevents legendary gems from being strictly superior -- they trade one flexible slot for one predetermined synergy.

---

## Gem Rarity

| Rarity    | Drop Rate | Modifier Slots | Visual Tier                        |
|-----------|-----------|----------------|------------------------------------|
| Common    | 60%       | 2              | Subtle glow                        |
| Uncommon  | 25%       | 2              | Steady glow + particle trail       |
| Rare      | 10%       | 3              | Bright glow + particle burst       |
| Epic      | 4%        | 3              | Aura + environmental interaction   |
| Legendary | 1%        | 3 + 1 unique   | Full visual transformation         |

### Expected Drop Counts

Assuming a dungeon run yields ~8 gem drops:

| Rarity    | Expected per Run | Expected Runs for 1 Drop |
|-----------|------------------|--------------------------|
| Common    | 4.8              | 1                        |
| Uncommon  | 2.0              | 1                        |
| Rare      | 0.8              | 1.25                     |
| Epic      | 0.32             | 3.13                     |
| Legendary | 0.08             | 12.5                     |

---

## Gem Acquisition

### Dungeon Drops (Targeted)

Specific dungeons drop specific gem families. Players farm with intent, not hope.

- Storm gems: Tempest Spire, Shattered Reach
- Nature gems: Verdant Hollow, Rootmaw Depths
- Void gems: Abyssal Rift, Eclipsed Sanctum

### Crafting (Controlled Randomness)

Crafting uses controlled randomness with player agency. Players use catalysts and techniques to bias results, but the mutation-driven gem system means perfect control is impossible. If you have the materials and skill, you get the gem — but mutation outcomes during use are where variance lives.

- Materials sourced from salvaging unwanted gems + dungeon-specific reagents
- Higher rarity recipes require rarer reagents and higher crafting level
- **Legendary gems are craft-exclusive** — they never drop from any content (see [[Loot System]])

### NPC Marketplace (No P2P Trading)

All gems are tradeable through the NPC-mediated marketplace. No direct player-to-player trading. Sellers list gems with NPCs who appraise and price them; buyers browse and purchase anonymously. This eliminates RMT by design. See [[Trade System]] for full specification.

### Mastery Teaching (Level 10 Mastery)

Players who reach Mastery 10 on a gem can create tradeable copies. This rewards expertise and feeds the economy with supply.

### Quest Rewards (Guaranteed)

Specific quests grant specific gems. New players are never stuck without core tools.

---

## Gem Mastery

Mastery levels 1 through 10, earned through active use of the gem.

### Mastery XP Formula

```
Mastery_XP_Earned = Base_XP * Content_Diversity_Multiplier * Alignment_Bonus
```

Where:

- **Base_XP**: XP earned per combat encounter or skill usage event
- **Content_Diversity_Multiplier**: Bonus for using the gem across different content types
  - 1 content type: 1.0x
  - 2 content types: 1.15x
  - 3 content types: 1.35x
  - 4+ content types: 1.50x
  - Content types: PvE dungeons, PvP arenas, world bosses, crafting support, exploration
- **Alignment_Bonus**: 1.0 (neutral), 1.10 (aligned with gem element), 0.90 (opposed)

### Mastery Progression Target

| Mastery Level | Cumulative Hours (Estimated) | Power Multiplier |
|---------------|------------------------------|------------------|
| 1             | 0                            | 1.000            |
| 2             | 2                            | 1.045            |
| 3             | 5                            | 1.100            |
| 4             | 9                            | 1.165            |
| 5             | 14                           | 1.240            |
| 6             | 19                           | 1.325            |
| 7             | 24                           | 1.420            |
| 8             | 29                           | 1.525            |
| 9             | 34                           | 1.640            |
| 10            | 40                           | 1.765            |

Power multiplier derived from:

```
Power_Multiplier = 1 + 0.04 * M + 0.005 * M^2
```

where M = Mastery Level. See [[Gem System Math]] for full derivation.

Mastery 10 requires approximately **40 hours of active use across diverse content**. This is intentionally long -- mastery is a statement of commitment, not a checkbox.

### Mastery Echo

A gem permanently destroyed in high-risk content ([[Death & Defeat System]]) leaves a **Mastery Echo** on the character for that domain + skill.

- The next gem of the same skill the player shapes or acquires starts at **50% of the destroyed gem's mastery XP**.
- Echoes don't stack. A new echo replaces the old one only if it is higher.
- *Why:* Mastery 10 is about 40 hours. Losing all of it to one bad round is the PKO-style compounding loss the avoid list rules out. The echo keeps breakage meaningful (half the investment is gone, plus the gem itself) without erasing a player's history.

---

## Gem Interaction Rules

1. **Element Compatibility**: Modifier gems must be compatible with the skill gem's element. Each modifier lists its compatible elements.
2. **Cross-Element Modifiers**: Exist but are rarer (Rare tier and above). Allow hybrid builds at a cost.
3. **Conversion Limit**: Maximum one "Conversion" modifier per skill gem. This prevents element-stacking exploits where a single skill would proc multiple elemental effects.
4. **Alignment Interaction**: The player's alignment modifies ALL equipped gems simultaneously. Switching alignment changes every skill's behavior. See [[Alignment System]] for alignment depth mechanics.
5. **No Duplicate Modifiers**: You cannot slot the same modifier gem twice on a single skill gem.
6. **Cross-Skill Duplicates Allowed**: The same modifier type can appear on different skill gems within a build.

## Companion Gem (Post-MVP)

> [!info] Source
> Adapted from PKO's fairy pets ([[PKO - Social, Endgame & Monetization]]): slow passive growth and breeding. PKO's premium-currency feeding, failed level-ups and paid breeding fruit are left out.

A companion is a gem in its own slot. It is a bonded creature or construct that travels with the character.

| Property | Rule |
|---|---|
| Slots | 1, separate from the 4 Skill Gem slots |
| Stats | ~8 stat points (a quarter of a Skill Gem) in its domain spread ([[Stats System#Stat Contribution Per Gem Domain]]) |
| Traits | 1 active trait (once per fight, e.g. *Scout*: reveal one extra enemy intent) + 1 passive trait (e.g. *Mender*: +3% Integrity regen per round) |
| Power budget | Counts **inside** the 80-100 build budget, Utility/Support axes only, ≤5 points ([[Gem System Math#Power Budget Per Build]]) |
| Stability | None. Companions never break, so they are the safe long-term investment |
| Enables | The [[Modifier Gems Reference|Companion Echo]] modifier |

### Bond (Growth Meter)

- Bond 1-10, filled by rounds fought with the companion equipped. Reaching Bond 10 takes about 20 hours.
- Bond raises trait potency from 100% to 150%. It never adds stats.
- **No feeding, no consumables, no failure chance, nothing purchasable.** Growth comes only from playing.

### Companion Fusion

- Two Bond 10 companions are fused at a Gem Shaper ([[Crafting System#Gem Shaping]]).
- The player **chooses** one trait from each parent for the offspring. The result is deterministic, with no RNG.
- The offspring starts at Bond 1, and both parents return to Bond 5. Fusing costs time, not the companions themselves.
- Rare trait combinations can be published to the Resonance Journal, as with resonances.

### Monetization

Companion skins are cosmetic-only shop items ([[Cosmetic System]]). Bond, traits and fusion can never be bought ([[Monetization#What We Will NEVER Sell]]).

---

## Design Intent

The gem system exists to make build-crafting the primary endgame activity. Players should spend more time thinking about gem combinations than grinding for power. The deterministic crafting path and full trading economy ensure that time-to-build is measured in decisions, not in luck.

The combinatorial space is enormous by design. See [[Gem System Math]] for the full analysis.

---

## Related Pages

- [[Gem System Math]] -- Damage formulas, balance calculations, DPS modeling
- [[Skill Gems Reference]] -- Domain actions reference with base power and alignment variants
- [[Modifier Gems Reference]] -- Complete list of modifier gems with values
- [[UI Design]] -- Gem UI hierarchy, text colors, and readability standards
