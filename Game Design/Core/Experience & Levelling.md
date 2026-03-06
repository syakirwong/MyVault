---
tags:
  - gdd
  - core
  - progression
  - levelling
---

# Experience & Levelling

## Design Philosophy

Levelling is a **teaching tool**, not a power treadmill. The journey from 1 to 50 exists to introduce systems, not to gate power. A new player reaching level 50 should understand gem domains, mutation, resonance, momentum, alignment, and the economy — because the level curve taught them each system in sequence.

The level cap is reachable. The real game begins at 50. Everything before it is the longest tutorial ever designed to not feel like one.

---

## Origins (Replaces Classes)

> **Design Note:** The [[GDD Hub]] states "Origin: 20% of build identity." The [[Gem Identity System]] states "There are no classes." **Origins** are the resolution — a one-time character creation choice that provides starting stat bias and a single passive ability, but does NOT restrict gem access. Origins are your history, not your destiny.

### 6 Origins

| Origin | Starting Stat Bias | Passive Ability | Lore |
|---|---|---|---|
| **Stormborn** | +15 Force, +10 Precision | **Gale Step:** +1 speed priority on first round of combat | Born during a tempest, raised by the sea |
| **Voidtouched** | +15 Acuity, +10 Attunement | **Hollow Gaze:** Debuffs you apply reveal 1 additional piece of opponent info | Marked by the void in childhood |
| **Ironsworn** | +15 Resilience, +10 Force | **Unbreaking:** First time Integrity drops below 20%, gain +5 stability on all gems | Forged in discipline and endurance |
| **Wavereader** | +15 Attunement, +10 Acuity | **Echo Sense:** Resonance trigger chance +5% passively | Attuned to the world's deeper rhythms |
| **Drifter** | +15 Instinct, +10 Precision | **Improvise:** Draw +1 action on the first round of any fight | Survived by reading situations fast |
| **Shaper** | +15 Precision, +10 Attunement | **Maker's Eye:** Crafted items have +3% quality bonus | Sees the world as raw material |

**Rules:**
- Origin is chosen at character creation and is **permanent**
- Origin stat bias is a flat bonus — it does not scale with level
- Origin passive is always active — it cannot be unequipped
- Origin does NOT restrict gem access. A Stormborn can equip Aegis + Synthesis just as well as an Ironsworn
- Origin contributes ~20% of identity through stat bias and passive, exactly as the GDD Hub specifies

---

## XP Sources

### Primary Sources

| Source | Base XP | Notes |
|---|---|---|
| Dungeon completion | 800-2,000 | Scales with dungeon tier. Mythic grants 2x |
| Dungeon boss kill | 300-600 | Per boss. Bonus for first kill |
| PvP match (win) | 400-800 | Scales with opponent level difference |
| PvP match (loss) | 150-300 | Always some reward for showing up |
| World boss contribution | 500-1,200 | Scales with contribution %, not damage dealt |
| Quest completion | 200-3,000 | Story quests grant the most |

### Secondary Sources

| Source | Base XP | Notes |
|---|---|---|
| Exploration discovery | 100-500 | First-time zone discovery, hidden locations |
| Crafting (first recipe) | 150-400 | Only first craft of each recipe grants XP |
| Trade route completion | 200-600 | Post-MVP content |
| Daily bounty | 300-500 | Capped at 3 bounties/day for XP |
| Alignment quest | 400-1,000 | Alignment-specific story content |
| Resonance discovery | 250 (flat) | First time discovering any resonance |

### Anti-Exploit Rules

- **AFK XP:** Zero. XP requires active combat participation or quest completion
- **Level disparity:** Killing enemies 10+ levels below grants 0 XP
- **Repeat farming:** Same dungeon in the same session grants diminishing XP (100% → 75% → 50% → 25% → 10%)
- **Party XP:** Full XP for all party members. No splitting. Group play is never punished

---

## XP Curve

### Formula

```
XP_to_next_level = 500 * Level^1.3
```

### Level Table

| Level | XP to Next | Cumulative XP | Estimated Hours | Key Unlock |
|---|---|---|---|---|
| 1 | 500 | 0 | 0 | Game start. First gem acquired |
| 5 | 3,700 | 9,800 | 0.5 | 2nd gem slot. Basic crafting |
| 10 | 10,000 | 42,000 | 1.5 | 3rd gem slot. PvP unlocked |
| 15 | 17,600 | 105,000 | 3 | 4th gem slot (full loadout). Stormreach access |
| 20 | 26,200 | 203,000 | 5 | Modifier gem slots expand to 3. First faction intro |
| 25 | 35,600 | 340,000 | 7 | Alignment system unlocked (choice at level 25) |
| 30 | 45,800 | 520,000 | 9.5 | Mythic dungeons accessible. Overcharge unlocked |
| 35 | 56,600 | 746,000 | 12 | Build publishing. Conditional actions unlocked |
| 40 | 68,000 | 1,022,000 | 14.5 | Faction reputation system fully active |
| 45 | 79,800 | 1,352,000 | 17 | Endgame content accessible early |
| 50 | — | 1,740,000 | 20 | **Soft cap.** All content accessible |

### Pacing Intent

- **Levels 1-15 (~3 hours):** One level every 10-15 minutes. Rapid unlocks keep momentum. Each level teaches a new mechanic
- **Levels 15-30 (~6.5 hours):** One level every 25-35 minutes. Settling into the session loop
- **Levels 30-50 (~10.5 hours):** One level every 45-60 minutes. Meaningful progression milestones, not just numbers ticking up
- **Total to 50: ~20 hours** — matches the target in [[Progression Systems]]

---

## Prestige Levels (50-60)

Prestige levels are earned after the soft cap. They are **cosmetic + minor** and exist for long-term identity, not power.

### Prestige XP

```
Prestige_XP_to_next = 100,000 * (Prestige_Level - 49)
```

Prestige 51: 100,000 XP. Prestige 60: 1,100,000 XP. Total for all 10 prestige levels: ~6,000,000 XP (~80+ hours of endgame play).

### Prestige Rewards

| Prestige Level | Reward |
|---|---|
| 51 | Title: "Veteran" + nameplate border upgrade |
| 52 | +1% all stats (flat) |
| 53 | Prestige combat log style (subtle visual) |
| 54 | +1% all stats (cumulative 2%) |
| 55 | Exclusive aura: "Stormbound" effect |
| 56 | +1% all stats (cumulative 3%) |
| 57 | Prestige build card border |
| 58 | +1% all stats (cumulative 4%) |
| 59 | Title: "Legend" + unique nameplate animation |
| 60 | +1% all stats (cumulative **5% max**). Title: "Paragon of [Origin]" |

**Hard rule:** Total prestige stat bonus never exceeds 5%. A prestige 60 player has at most a 5% stat edge over a fresh level 50 player. This gap is closeable through build quality alone.

---

## Rested XP

- Accumulates while logged out: **5% bonus XP per 8 hours offline**, capping at **50% bonus**
- Rested XP applies to primary sources only (combat, quests)
- Rested XP depletes as you earn XP with the bonus active
- **Purpose:** Respects players who can't play daily. A weekend warrior with full rested XP closes the gap with a daily player

---

## Level Scaling in Group Content

- **Downscaling:** High-level players in low-level content have stats reduced to zone-appropriate levels. Gem loadout remains — you keep your options, not your numbers
- **Upscaling:** Low-level players in high-level content receive a stat floor but reduced rewards. Viable for playing with friends, not for power-levelling
- **PvP:** All PvP is normalized to level 50 stats. Level advantage does not exist in PvP. Only build quality matters

---

**Related:** [[Progression Systems]] | [[Stats System]] | [[Factions System]] | [[Gem Identity System]] | [[Combat System]]