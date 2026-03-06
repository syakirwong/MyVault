---
tags:
  - gdd
  - content
  - pve
---

# PvE Structure

## Design Philosophy

Dungeons are the economic and progression engine. The open world is the connective tissue — exploration, travel, resource gathering — but progression lives in dungeon mastery. Every dungeon run should feel like a build test, not a chore.

Players aren't grinding levels. They're grinding possibilities.

---

## Dungeon Categories

Dungeons have four distinct roles. Each serves a different player need and session length. Mixing them into a single category creates content that feels identical with different textures.

### 1. Expedition Dungeons (Main Content)

The core PvE experience. Tactical group content where build synergy matters.

| Property | Detail |
|---|---|
| Party Size | 3-5 players |
| Duration | 20-30 minutes |
| Difficulty | Medium-High |
| Focus | Boss mechanics, build synergy, coordination |

**Rewards:**
- Rare and Epic gems
- Equipment drops
- Crafting components (high-tier)

**MVP Expedition Dungeons (5):**

| Dungeon | Region | Element | Bosses | Primary Drops |
|---|---|---|---|---|
| Tidecaller Caverns | Calm Shores | Water/Nature | 2 | Aegis gems, defensive modifiers |
| Stormspire | Stormreach | Storm | 3 | Tempest gems, damage modifiers |
| Thornwood Hollow | Verdant Depths | Nature | 2 | Synthesis gems, utility modifiers |
| Void Nexus | Obsidian Wastes | Void | 3 | Cross-domain gems, elemental modifiers |
| Radiant Sanctum | Celestial Peaks | Light | 3 | Synergy modifiers, Epic gems |

### 2. Solo Trials (Skill Expression)

Pure mechanical tests. One player, one build, no carries. This is where the [[Creator Ecosystem]] feeds — trials generate leaderboard content and build showcase moments.

| Property | Detail |
|---|---|
| Party Size | 1 player |
| Duration | 10-15 minutes |
| Difficulty | High (mechanical) |
| Focus | Boss mechanics, execution, build optimization |

**Rewards:**
- Cosmetic auras (trial-specific)
- Rare modifier gems
- Leaderboard ranking
- Mastery XP bonus (1.5x for trial completion)

Solo Trials rotate weekly. Each trial tests a different combat axis — one week rewards burst damage, the next rewards sustained survivability, the next rewards mobility and positioning.

### 3. Farming Dungeons (Material Loops)

Short, repeatable loops with targeted drop tables. These feed the [[Crafting System]] and [[Trade System]]. Players know exactly what they're farming and why.

| Property | Detail |
|---|---|
| Party Size | 1-3 players |
| Duration | 5-10 minutes |
| Difficulty | Low-Medium |
| Focus | Speed, efficiency, targeted drops |

**Rewards:**
- Specific gem families (targeted, not random)
- Crafting materials
- Salvage fodder

**MVP Farming Dungeons (5):**

| Dungeon | Element Focus | Primary Drops |
|---|---|---|
| Storm Cavern | Storm | Lightning gems, Storm reagents |
| Ash Volcano | Fire | Fire modifiers, Ignite components |
| Tidal Pools | Water/Nature | Healing gems, Nature reagents |
| Shadow Grotto | Void | Void gems, Conversion materials |
| Windswept Ruins | Wind | Wind modifiers, Mobility reagents |

### 4. Mythic Dungeons (Social Hype Events)

Rare, rotating dungeons with unique mechanics and visuals. These create the "did you see that?" moments that drive community engagement. Limited availability creates urgency without FOMO — they rotate, they don't disappear.

| Property | Detail |
|---|---|
| Party Size | 5 players |
| Duration | 30-45 minutes |
| Difficulty | Very High |
| Focus | Unique mechanics, spectacle, prestige |
| Availability | Rotating — one Mythic active per week |

**Rewards:**
- Legendary Components (rare materials and catalysts required to craft Legendary gems — Legendaries are craft-exclusive, see [[Loot System]])
- Epic gems (higher floor than Expeditions)
- Mythic-tier crafting materials
- Mythic cosmetic effects (visual transformations)
- Unique titles
- Guild prestige points

---

## Challenge Mode

Expedition Dungeons have a Challenge Mode overlay:

- Same dungeon layout, modified rules (time pressure, buffed enemies, restricted gems, environmental hazards)
- Weekly rotation of modifiers
- Leaderboard for fastest clears per build archetype
- Rewards: Rare/Epic gems, cosmetic dungeon titles

Challenge Mode is where the meta gets tested. Weekly modifier rotations force build adaptation — last week's optimal build may be this week's worst pick.

---

## Boss Design Philosophy

Since combat is built on commitment and consequence (see [[Combat System]]), bosses must respect the same rules.

### Bosses Must:

- **Telegraph attacks** — every major attack has a readable windup. The punishment is for failing to respond, not for failing to guess
- **Reward positioning** — flanking, backstab, and zone control matter against bosses, not just players
- **Have phases** — HP thresholds trigger new mechanics. Each phase tests a different combat skill
- **Punish greed** — overcommitting to damage during a boss telegraph should result in taking the hit. Recovery frames matter here
- **Enable counterplay** — at least one boss attack per encounter should be parryable for a stagger window

### Bosses Must NOT:

- Be HP sponges — if the fight is long, it should be because mechanics demand careful play, not because the health bar is inflated
- Require specific builds — any composition should be able to clear, with efficiency varying by synergy
- Have unavoidable damage as the primary threat — damage should come from mechanical failure, not passive auras

### Example Boss: Storm Leviathan (Stormspire, Boss 3)

**Phase 1 (100-60% HP): Thunder Strikes**
- Telegraphed lightning pillars target random players (2s warning, dodge or take heavy damage)
- Tail sweep (parryable) — successful parry staggers and exposes a backstab window
- Chain lightning between players standing too close — spacing matters

**Phase 2 (60-30% HP): Charged Ground**
- Arena floor becomes electrified in zones — players must position on safe ground
- Leviathan charges across the arena (dodge or take massive damage + stun)
- Lightning rods spawn — players must ground electricity by standing near rods, creating safe zones for the party

**Phase 3 (30-0% HP): Storm Convergence**
- All previous mechanics active simultaneously
- Leviathan gains an interruptible channel — if not interrupted, arena-wide damage wipe
- Enrage soft timer: storm intensity increases every 30s, shrinking safe ground

---

## Open World Purpose

The open world supports three functions. It is not the progression engine — dungeons are.

### 1. Exploration
- Hidden mini-dungeons (short, 5-minute diversions with minor rewards)
- Treasure maps leading to cosmetic or lore rewards
- Environmental storytelling and lore collectibles
- Secret areas that reward curiosity

### 2. Travel and Connection
- Dungeon entrances exist in the world (see Queue System below)
- [[Social Hubs]] are physical locations players pass through
- Travel between regions creates a sense of scale
- Fast travel unlocked per region after exploration milestones

### 3. Resource Gathering
- Crafting materials from world nodes (not dungeon-exclusive)
- Rare world bosses on 2-hour spawn timers (10-50 players)
- Trade route paths with ambient PvP risk zones (opt-in)
- Daily bounties (15-20 min, repeatable)

### World Regions (5 at MVP)

| Region | Element | Level Range | Key Content |
|---|---|---|---|
| Calm Shores | Water/Nature | 1-10 | Tutorial, Tidecaller Caverns |
| Stormreach | Storm | 10-25 | Stormspire, PvP arena |
| Verdant Depths | Nature | 20-35 | Thornwood Hollow, world boss |
| Obsidian Wastes | Void | 30-45 | Void Nexus, corruption mechanics |
| Celestial Peaks | Light | 40-50 | Radiant Sanctum, endgame hub |

---

## Queue System

Dungeons are the main event. Queue friction kills engagement. The system uses a **hybrid model**:

- **Queue from anywhere** — players can queue for any unlocked dungeon while exploring, trading, or socializing
- **Teleport on party ready** — when all party members accept, the group teleports to the dungeon entrance
- **Physical entrances exist** — players near a dungeon entrance can enter directly without queue
- **Return to origin** — after dungeon completion, players return to their pre-queue location

This preserves world immersion (the world isn't empty because everyone is in menus) while eliminating wait frustration.

---

## Endgame Content Sustainability

### The Core Problem

If dungeons are the progression engine, content exhaustion is the existential risk. Players will ask "how many dungeons are there?" and the answer must never feel like "not enough." A dungeon-driven MMO lives or dies on replayability.

Fully handcrafted dungeons produce high quality but limited quantity. Fully procedural dungeons solve quantity but destroy identity — "Stormspire" means nothing if it's different every time, and procedural bosses can't deliver the telegraphed, phase-based, parry-window encounters the [[Combat System]] demands. Procedural generation also kills the [[Creator Ecosystem]] — you can't publish "a build for this dungeon" if the dungeon changes.

### Solution: Hybrid Dungeon Model

Handcrafted shells with procedural variation layers.

| Layer | Fixed (Authored) | Variable (Procedural) |
|---|---|---|
| Boss encounters | Yes — scripted phases, mechanics, telegraphs | No — bosses are always hand-designed |
| Key rooms | Yes — boss arenas, set-piece moments | No — signature rooms define dungeon identity |
| Visual identity | Yes — art direction, theming, atmosphere | No — dungeons must be visually recognizable |
| Lore and story | Yes — environmental storytelling, NPC dialogues | No — narrative is authored |
| Connector rooms | Partially — authored room templates | Yes — 3-5 layout variants per section, randomized |
| Trash mob composition | Partially — element theme maintained | Yes — mob types, pack sizes, and positioning vary |
| Trap/hazard placement | Partially — authored hazard types | Yes — placement and combinations vary per run |
| Drop table emphasis | No | Yes — weekly rotation shifts which gems are weighted |

### Variation Sources

Each source of variation maps to an existing system:

| Variation Source | System | Effect on Replayability |
|---|---|---|
| Challenge Mode modifiers | Weekly rotation | Rule changes force build adaptation per dungeon |
| Gem drop table rotation | [[Gem System]] | Different gems emphasized each week, shifting farming targets |
| Mob element composition | World Design | Some weeks Storm-heavy trash, some Void-heavy — builds must adapt |
| Room connector layouts | New (templated) | 3-5 variants per dungeon section, procedurally selected |
| Trap/hazard placement | New (templated) | Environmental variety within fixed boss arenas |
| Mythic dungeon rotation | Weekly cycle | One Mythic active per week, cycling through the full pool |

### Effective Content Multiplier

With the hybrid model, each dungeon has 4-6 distinct "feels" before repeating. Combined with weekly Challenge Mode modifiers:

```
Effective experiences = Dungeon count * Layout variants * Modifier rotation
```

At year one totals: 13 Expeditions * 5 variations = ~65 distinct Expedition experiences per modifier cycle. This is sustainable.

### Content Pipeline Targets

| Phase | Expedition | Farming | Mythic | Solo Trials | Total |
|---|---|---|---|---|---|
| MVP | 5 | 5 | 2 | 3 | 15 |
| 6-month update | +3 | +2 | +1 | +2 | +8 |
| 12-month expansion | +5 | +3 | +2 | +3 | +13 |
| **Year 1 total** | **13** | **10** | **5** | **8** | **36** |

### What This Means for Development

- **Boss design is the bottleneck** — every Expedition and Mythic boss is fully authored. Budget development time for boss encounters first, then build dungeon shells around them
- **Room templates are the force multiplier** — investing in a robust room-template system early pays dividends on every future dungeon. A library of 50 connector room templates can serve dozens of dungeons
- **Modifier design is cheap content** — each new Challenge Mode modifier creates a new experience across every existing dungeon. A single well-designed modifier is effectively 13+ new dungeon runs
- **Mob variety scales naturally** — as new enemy types are added for world content, they feed into the procedural trash variation pool for all dungeons

### Endgame Progression Path

Once a player reaches max level and has cleared all Expedition dungeons:

```
Challenge Mode mastery (push leaderboards)
→ Solo Trial ranking (skill expression)
→ Mythic dungeon clears (prestige + Legendary gems)
→ Raid progression (post-MVP, group mastery)
→ Build optimization (gem mastery, modifier experimentation)
→ Creator ecosystem (publish builds, earn reputation)
→ PvP seasons (competitive expression)
```

Multiple endgame tracks running in parallel prevent any single track from being the sole source of engagement. When a player exhausts one track for the week, another is waiting.

---

## Raids (Post-MVP)

- 10-player raids, 60-90 minutes
- Require diverse team composition
- Legendary gem components drop here
- 2 raids planned for first expansion

---

## World Bosses

- Open-world, anyone can join
- Scale with participant count (10-50 players)
- Guaranteed reward for meaningful contribution (damage, healing, tanking thresholds)
- Weekly featured world boss with bonus drops

---

## Related Pages

- [[Combat System]] -- Moment-to-moment combat mechanics
- [[Gameplay Loops]] -- Session and meta loop structure
- [[Gem System]] -- Build customization backbone
- [[Progression Systems]] -- Character advancement
- [[Creator Ecosystem]] -- Build sharing and community content
