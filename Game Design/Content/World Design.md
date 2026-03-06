---
tags:
  - gdd
  - content
  - world
---

# World Design

## World Overview

The game world is an archipelago — a chain of islands connected by sea routes. Regions are presented through descriptive text, ambient UI elements, and contextual action buttons rather than visual maps. Each region has a distinct textual atmosphere, dominant gem domain ecology, faction presence, and unique gameplay offerings.

---

## Gem Domain Ecology

Every region has a **dominant domain** and a **secondary domain** that shape the environment. These are not cosmetic — they have mechanical effects on combat and gem behavior.

| Effect | Description |
|--------|-------------|
| **Domain Resonance Bonus** | Gems matching the zone's dominant domain start combat at +5 stability |
| **Domain Suppression** | The domain opposing the zone's dominant loses -5 starting stability |
| **Environmental Hazards** | Dungeon hazards in the zone deal damage typed to the dominant domain |
| **Wild Gem Drops** | Open-world gem drops skew toward the zone's dominant and secondary domains |
| **Mutation Pressure** | Gems of the dominant domain mutate 20% faster in this zone (more instability per use) |

### Domain Opposition Map

| Domain | Opposed By |
|--------|-----------|
| Kinetics | Void |
| Tempest | Aegis |
| Entropy | Synthesis |
| Flux | Resonance |

---

## Geography

### Calm Shores (Starter Island)

- **Theme:** Tropical harbor, trade hub, safe haven
- **Level:** 1-10
- **Dominant Domain:** Aegis | **Secondary:** Synthesis
- **Atmosphere:** Welcoming — warm breeze, bustling docks, gulls calling, the hum of freshly cut gems from Tova's workshop
- **Faction Presence:**
  - [[Factions System#Port Haven Trade Guild|Port Haven Trade Guild]] (HQ) — Harbormaster Finn runs the NPC Marketplace
  - All three alignment NPCs present (Lightkeeper Asha, Voidcaller Myx, Wanderer Syl)
- **Key Locations:**
  - **Port Haven** — Main social hub (see [[Social Hubs#Port Haven]]). Trade center, guild recruitment, build inspection
  - **Training Grounds** — Sparring Master Holt's practice arena. Gem system tutorial, build testing dummies
  - **Tidecaller Caverns** — First expedition dungeon (Water/Nature themed, Aegis-domain ecology)
  - **Shallow Caves** — Farming dungeon (introductory, teaches dungeon flow)
- **Purpose:** Tutorial, first gem acquisition, gem system introduction, basic combat mastery. Captain Maren Voss guides new players through "The First Spark" questline
- **Domain Ecology Effect:** The Aegis-dominant environment makes this zone forgiving. Defensive gems are slightly stronger here, easing new players into the system before they encounter harsher domain ecologies

### Stormreach Archipelago

- **Theme:** Volcanic islands with perpetual storms, competitive edge
- **Level:** 10-25
- **Dominant Domain:** Tempest | **Secondary:** Kinetics
- **Atmosphere:** Intense — thunder rumbles, lightning arcs between volcanic peaks, sulfur in the air, the clash of arena combat echoes from Thunder Arena
- **Faction Presence:**
  - [[Factions System#Stormreach Arena League|Stormreach Arena League]] (HQ) — Arena Warden Zara manages ranked PvP
  - [[Factions System#The Gemcutter's Circle|The Gemcutter's Circle]] (HQ) — Lumen runs resonance research at Lightning Forge
  - Admiral Kael Stormwright commands from Stormreach Command Tower
- **Key Locations:**
  - **Stormspire** — Expedition dungeon (3 bosses, Tempest ecology). Storm Leviathan is the signature boss encounter
  - **Lightning Forge** — Crafting hub. Lumen's resonance mapping workshop, advanced gem shaping
  - **Thunder Arena** — PvP social hub (see [[Social Hubs#Thunder Arena]]). Arena queue, leaderboards, honor vendor, spectator mode with [[Combat Logs System|enhanced combat logs]]
  - **Storm Cavern** — Farming dungeon (Tempest gems, Storm reagents)
- **Purpose:** Combat mastery, Tempest and Kinetics domain acquisition, PvP introduction, competitive play
- **Domain Ecology Effect:** Tempest-dominant environment rewards aggressive chain-building. Gems with chain mechanics thrive here. Aegis gems suffer suppression — defensive players must adapt or struggle

### Verdant Depths

- **Theme:** Overgrown jungle islands with ancient ruins, hidden knowledge
- **Level:** 20-35
- **Dominant Domain:** Synthesis | **Secondary:** Resonance
- **Atmosphere:** Mysterious — canopy whispers, bioluminescent paths, ancient carvings pulse with construct energy, the Driftmind's presence permeates the roots
- **Faction Presence:**
  - [[Factions System#The Driftborn|The Driftborn]] (HQ) — The Driftmind resides at Druid Circle, headquarters for the Unbound alignment
  - Vaela Thornstitch runs catalyst brewing from her workshop
- **Key Locations:**
  - **Thornwood Hollow** — Expedition dungeon (2 bosses, Synthesis/Resonance ecology). Construct-heavy encounters reward build patience
  - **Druid Circle** — Driftborn headquarters. Unbound alignment questline ("The Third Path"), Vaela's catalyst workshop
  - **Canopy Market** — Regional trade hub. Rare material vendors, cross-faction trading post
  - **Tidal Pools** — Farming dungeon (healing gems, Nature reagents, Synthesis components)
  - **World Boss Arena** — Open clearing where the Thornmother spawns (2-hour timer, 10-50 players)
- **Purpose:** Exploration, Synthesis and Resonance domain acquisition, crafting material gathering, Driftborn faction progression
- **Domain Ecology Effect:** Synthesis-dominant environment favors construct-building and echo strategies. Constructs last 1 round longer in this zone. Entropy gems face suppression — decay strategies are weakened in the living jungle

### Obsidian Wastes

- **Theme:** Corrupted volcanic badlands, power at a price
- **Level:** 30-45
- **Dominant Domain:** Entropy | **Secondary:** Void
- **Atmosphere:** Oppressive — ground hums with decay energy, shadows shift and consume, the air tastes of dissolution. The Hollow King's influence saturates every surface
- **Faction Presence:**
  - [[Factions System#The Hollow Court|The Hollow Court]] (HQ) — The Hollow King holds court at Corruption Font
  - Voidcaller Myx maintains a permanent presence at the Shadow Bazaar
- **Key Locations:**
  - **Void Nexus** — Expedition dungeon (3 bosses, Entropy/Void ecology). Gem stability drains faster here — mutation management is the core challenge
  - **Corruption Font** — Hollow Court headquarters. Corrupted alignment questline ("The Price of Power"), alignment depth progression
  - **Shadow Bazaar** — Black market trade hub. Rare Void and Entropy gems, high-risk trade route terminus
  - **Shadow Grotto** — Farming dungeon (Void gems, conversion materials)
  - **Rift Zones** — Temporary open-world areas where void corruption surges. Enhanced drops but ambient stability drain on all equipped gems
- **Purpose:** Endgame transition, Entropy and Void domain mastery, Hollow Court faction progression, alignment commitment
- **Domain Ecology Effect:** Entropy-dominant environment accelerates all decay. Debuffs last longer, stability drains faster, mutation thresholds are reached sooner. Synthesis gems face heavy suppression — constructs crumble faster. This zone demands aggressive, decisive play

### Celestial Peaks

- **Theme:** Floating islands above the clouds, achievement and legacy
- **Level:** 40-50+
- **Dominant Domain:** Resonance | **Secondary:** Flux
- **Atmosphere:** Ethereal — wind chimes from crystal formations, air tastes of pure attunement, the horizon stretches forever, and every action seems to echo with deeper meaning
- **Faction Presence:**
  - [[Factions System#The Luminari|The Luminari]] (HQ) — High Luminar Sera presides at Radiant Sanctum
  - Price Historian Dael provides market analysis at Sky Forum
  - Rune (Post-MVP) operates gem salvaging from Crafter's Spire
- **Key Locations:**
  - **Radiant Sanctum** — Expedition dungeon (3 bosses, Resonance/Flux ecology). Resonance-heavy encounters reward echo mastery and adaptation
  - **Sky Forum** — Endgame social hub (see [[Social Hubs#Sky Forum]]). Creator's Hall, build showcase, Legendary crafter storefronts, guild hall directory
  - **Creator's Hall** — Build sharing and display venue. Top builds featured on the build board, feeds the [[Creator Ecosystem]]
  - **Windswept Ruins** — Farming dungeon (Flux modifiers, mobility reagents)
  - **Mythic Portal** — Access point for the weekly Mythic dungeon rotation
- **Purpose:** Endgame hub, legendary content, social display, Luminari faction progression, resonance mastery
- **Domain Ecology Effect:** Resonance-dominant environment amplifies all echo effects. Echoes fire at +20% power. Resonance discovery chance is boosted. Flux gems thrive (adaptation is rewarded). This is where the deepest build experimentation happens

---

## Navigation

### Travel System

- **Fast travel** between discovered hubs (instant, gold cost scaling with distance)
- **Sea travel** between island chains (text-based journey with random encounter events — ambient combat, trade opportunities, lore discoveries)
- **Location list** with level recommendations, dominant domain indicator, and available content badges (dungeon, PvP, world boss, faction)
- **Region descriptions** update dynamically based on active world events, faction control state, and player alignment

### Queue System Integration

Dungeons use the hybrid queue model defined in [[PvE Structure#Queue System]]:

- **Queue from anywhere** — players can queue while exploring, trading, or socializing
- **Teleport on party ready** — group teleports to dungeon entrance when all accept
- **Physical entrances exist** — players near a dungeon can enter directly
- **Return to origin** — after completion, players return to their pre-queue location

This keeps the open world populated. Players aren't sitting in menus — they're in the world while waiting.

---

## Zone Contested Areas

Each region (except Calm Shores) contains one **Contested Zone** — an area where opt-in open-world PvP is active.

| Region | Contested Zone | Mechanic |
|--------|---------------|----------|
| Stormreach | Thunder Pass | Faction control points. Holding grants +5% XP to controlling faction in the zone |
| Verdant Depths | Thornwall Crossing | Trade route ambush zone. Cargo escorts pass through here (see [[Gameplay Loops#Trade Routes]]) |
| Obsidian Wastes | Rift Borderlands | Alignment territory. Radiant vs Corrupted push with shifting corruption boundaries |
| Celestial Peaks | Ascension Spires | High-level dueling grounds. Voluntary, prestige-focused. Enhanced combat log for spectators |

**Rules:**
- PvP in contested zones is **opt-in** (flag toggleable)
- Flagged players gain +15% gem drop rate and +10% faction reputation while in the zone
- Death in contested zones has no gem stability penalty (PvP deaths are consequence-light)
- Alignment Wars (weekly event) expand contested zones temporarily

---

## World Events

Events follow the cadence defined in [[Gameplay Loops#Weekly Loop]] and [[Factions System#Faction Events]].

### Weekly Events

| Event | Description | Affected Zones | Mechanical Effect |
|-------|-------------|---------------|-------------------|
| **Alignment Wars** | Major factions compete for territory control across contested zones | All contested zones | PvE and PvP objectives earn faction war points. Winning faction gains 5% XP zone bonus for the following week |
| **Mythic Rotation** | New Mythic dungeon becomes active, previous one rotates out | Mythic Portal (Celestial Peaks) | Community-wide hype, fresh boss encounters, Legendary component drops |
| **Challenge Mode Modifiers** | New modifier set applied to all Expedition dungeons | All dungeon zones | Forces build adaptation — last week's optimal build may fail this week |
| **Solo Trial Rotation** | New trial tests a different combat axis | Solo Trial arenas (all regions) | Burst, sustain, or mobility focus shifts weekly |
| **Trade Route Shifts** | Supply and demand changes across regions | All trade hubs | New arbitrage opportunities, material price fluctuations |

### Monthly Events

| Event | Description | Duration |
|-------|-------------|----------|
| **Trade Festival** | Boosted crafting yields, reduced AH fees, special limited-time recipes, festival cosmetics | 3 days |
| **Domain Surge** | One gem domain becomes empowered world-wide. Domain-matching gems gain +10 starting stability everywhere | 5 days |

### Seasonal Events

| Event | Description | Duration |
|-------|-------------|----------|
| **Season Championships** | Arena League tournament. Top 64 players, bracketed PvP with spectator mode and [[Combat Logs System|enhanced combat logs]] | 1-week qualifier + weekend finals |
| **Corruption Tides** | Void energy surges across all zones. Corrupted mobs spawn in non-corrupted areas, Rift Zones expand, unique Entropy/Void gems drop | 2 weeks |
| **Storm Convergence** | Tempest domain amplified globally. Weather events modify combat in all zones, Stormreach becomes the focal hub | 2 weeks |

### Event Communication

- Events announced through notification banners in all hubs
- Region atmosphere text updates to reflect active events ("The air crackles with unusual energy — a Domain Surge is active")
- World Boss Herald NPC roams to announce imminent spawns
- Combat logs reference environmental effects from active events

---

## Open World Purpose

The open world supports three functions. It is not the progression engine — [[PvE Structure|dungeons are]].

### 1. Exploration

- Hidden mini-dungeons (5-minute diversions with minor gem drops and lore)
- Treasure maps leading to cosmetic or lore rewards
- Environmental storytelling — ancient constructs, void scars, faction conflict remnants
- Secret areas that reward curiosity (rare ambient gem spawns, hidden NPC merchants)
- Domain ecology anomalies — pockets where the usual zone domain is overridden, creating unique combat conditions

### 2. Travel and Connection

- Dungeon entrances exist physically in the world
- [[Social Hubs]] are locations players pass through and linger in
- Sea travel between island chains creates a sense of scale
- Fast travel unlocked per region after exploration milestones
- Travel encounters during sea routes (ambient combat, trade opportunities, NPC encounters)

### 3. Resource Gathering

- Crafting material nodes scattered across zones (domain-typed, matching zone ecology)
- Rare world bosses on 2-hour spawn timers (10-50 players, scale with participant count)
- Trade route paths with ambient PvP risk in contested zones (opt-in)
- Daily bounties (15-20 min, repeatable, grant faction reputation for the zone's regional faction)

---

## Region Summary

| Region | Level | Dominant Domain | Secondary Domain | Faction HQ | Social Hub | Key Dungeon |
|--------|-------|----------------|-----------------|------------|------------|-------------|
| Calm Shores | 1-10 | Aegis | Synthesis | Trade Guild | Port Haven | Tidecaller Caverns |
| Stormreach | 10-25 | Tempest | Kinetics | Arena League, Gemcutter's Circle | Thunder Arena | Stormspire |
| Verdant Depths | 20-35 | Synthesis | Resonance | The Driftborn | Canopy Market | Thornwood Hollow |
| Obsidian Wastes | 30-45 | Entropy | Void | The Hollow Court | Shadow Bazaar | Void Nexus |
| Celestial Peaks | 40-50+ | Resonance | Flux | The Luminari | Sky Forum | Radiant Sanctum |

---

## Related Pages

- [[PvE Structure]] — Dungeon categories, boss design, queue system
- [[PvP Structure]] — Competitive modes and contested zone rules
- [[Combat System]] — Phase-based combat and environmental gem field effects
- [[Combat Logs System]] — How logs reflect zone ecology and world events
- [[Gem Identity System]] — 8 gem domains and their interactions
- [[Factions System]] — Faction headquarters, reputation, and territory control
- [[Alignment System]] — Alignment-specific content and zone interactions
- [[Social Hubs]] — Hub features and social systems per location
- [[NPC System]] — Named NPCs anchored to each region
- [[Gameplay Loops]] — Session, weekly, and meta loop structure
- [[Trade System]] — Trade routes and regional economy
