---
tags:
  - gdd
  - content
  - npc
---

# NPC System

## Design Philosophy

NPCs serve three purposes: anchor the world's narrative, provide essential services, and reinforce alignment identity. Every NPC should feel like they belong to a faction and have an opinion about the world's conflicts.

NPCs are NOT quest dispensers. They are characters with alignment, personality, and relationships to each other.

## NPC Categories

### Story NPCs

Major characters that drive narrative and world events.

#### Captain Maren Voss (Calm Shores)
- Role: Tutorial mentor, introduces gem system
- Alignment: Unbound
- Personality: Pragmatic retired pirate, dry humor
- Function: Guides player through Origin selection, first gem acquisition, basic combat
- Questline: "The First Spark" (tutorial chain, 1-2 hours)
- Location: Port Haven docks

#### High Luminar Sera (Celestial Peaks)
- Role: Radiant faction leader
- Alignment: Radiant (Paragon)
- Personality: Calm authority, protective, sees potential in everyone
- Function: Radiant alignment questline, endgame story
- Questline: "The Ascending Light" (Radiant story arc)
- Location: Radiant Sanctum

#### The Hollow King (Obsidian Wastes)
- Role: Corrupted faction leader
- Alignment: Corrupted (Paragon)
- Personality: Charismatic, philosophical about power, never overtly evil
- Function: Corrupted alignment questline, endgame story
- Questline: "The Price of Power" (Corrupted story arc)
- Location: Corruption Font throne room

#### The Driftmind (Verdant Depths)
- Role: Unbound faction leader
- Alignment: Unbound (Paragon)
- Personality: Enigmatic, speaks in questions, ancient forest entity
- Function: Unbound alignment questline, balance-related lore
- Questline: "The Third Path" (Unbound story arc)
- Location: Druid Circle

#### Admiral Kael Stormwright (Stormreach)
- Role: Regional authority, naval content introducer
- Alignment: Unbound (leaning Radiant)
- Personality: Bold, competitive, respects strength
- Function: Stormreach questline, introduces PvP and arena
- Questline: "Trial by Storm" (Stormreach main arc)
- Location: Stormreach Command Tower

### Service NPCs

Provide gameplay services. Each has a name and personality — no generic "Blacksmith" labels.

#### Crafting Masters

| NPC | Discipline | Region | Personality | Services |
|-----|-----------|--------|-------------|----------|
| Tova Ironhand | Gem Shaping | Port Haven | Gruff perfectionist, judges your materials | Gem crafting, rarity upgrades, domain reforging |
| Lumen | Resonance Mapping | Stormreach | Eccentric researcher, obsessed with gem resonance | Resonance probes, amplifiers, fragments, journal publishing |
| Vaela Thornstitch | Catalyst Brewing | Verdant Depths | Quiet, methodical, nature-inspired designs | Stabilizers, mutation catalysts, resonance catalysts |
| Rune (Post-MVP) | Salvaging | Celestial Peaks | Former gem wielder, channels through reclamation | Gem salvaging, fragment recovery, essence extraction |

Each crafting master:
- Teaches recipes as you level the discipline
- Has a personal questline (5-8 quests) that unlocks advanced recipes
- Comments on your crafting level and alignment
- Reacts to legendary items you've crafted ("Ah, I see you've surpassed me...")

#### Trade NPCs

| NPC | Function | Region | Notes |
|-----|----------|--------|-------|
| Harbormaster Finn | NPC Marketplace | Port Haven | Tracks your trade history, offers tips on gem prices |
| The Broker | Commission Board | All hubs | Mysterious figure, mediates crafter commissions, ensures fair deals |
| Caravan Master Ola | Trade route missions | Stormreach | Assigns trade route contracts, tracks route reputation |
| Price Historian Dael | Price charts and history | Sky Forum | Analytical, provides market commentary |

#### Alignment NPCs

| NPC | Alignment | Region | Function |
|-----|-----------|--------|----------|
| Lightkeeper Asha | Radiant | All major hubs | Alignment progress tracking, Radiant quests, alignment switch ritual |
| Voidcaller Myx | Corrupted | Shadow Bazaar + hubs | Corrupted quests, corruption gifts, alignment switch ritual |
| Wanderer Syl | Unbound | Roaming (changes location weekly) | Unbound quests, balance philosophy, alignment switch ritual |

Alignment NPCs:
- React to your current alignment depth
- Warn you about switching costs before committing
- Offer alignment-specific daily bounties
- Gate access to alignment-exclusive gems at appropriate depths (see [[Alignment System]])

#### Combat NPCs

| NPC | Function | Region |
|-----|----------|--------|
| Sparring Master Holt | Practice arena, build testing dummies | Port Haven |
| Arena Warden Zara | PvP queue, rankings, honor vendor | Thunder Arena |
| Dungeon Guide Pip | Dungeon finder, challenge mode activation | All dungeon entrances |
| World Boss Herald | Announces world boss spawns, contribution tracking | Roaming |

### Ambient NPCs

Background characters that make the world feel alive. They don't offer quests but react to players.

Behaviors:
- Comment on player alignment ("The void clings to you..." / "Your light is blinding.")
- React to legendary gear ("Is that a Tova Ironhand blade?")
- Discuss world events with each other
- Reference the current PvP season or top builds
- Flee from Corrupted players at deep alignment, cheer for Radiant

Types per hub:
- Merchants (sell basic consumables, price anchor items)
- Sailors and dockworkers (Calm Shores, Stormreach)
- Scholars and acolytes (Celestial Peaks)
- Guards (alignment-colored based on region)
- Children playing (humanize the world)

## NPC Behavior System

### Alignment Reactivity

NPCs change dialogue and behavior based on player alignment and depth.

| Depth | Radiant Reaction | Corrupted Reaction | Unbound Reaction |
|-------|-----------------|-------------------|-----------------|
| 0-20 | Neutral | Neutral | Neutral |
| 21-40 | Friendly greetings from Radiant NPCs | Wary looks from Radiant NPCs, nods from Corrupted | Respected by traders and neutrals |
| 41-60 | Radiant NPCs offer discounts (5%) | Guards become alert, Corrupted vendors unlock | Access to hidden neutral merchants |
| 61-80 | Radiant faction salutes you | Some NPCs refuse service, Corrupted NPCs reverent | Both factions seek you as mediator |
| 81-100 | Citizens gather to watch you pass | Citizens flee, dark ambient effects around you | Unique dialogue from ALL faction leaders |

### Reputation System

Separate from alignment. Earned through quests and interactions with specific NPC groups.

| Faction | Key NPCs | Reputation Rewards |
|---------|----------|-------------------|
| Port Haven Trade Guild | Harbormaster Finn, Caravan Master Ola | Reduced NPC marketplace fees, trade route bonuses |
| Stormreach Arena | Arena Warden Zara, Admiral Kael | PvP cosmetics, arena title upgrades |
| Gem Artisan Circle | Lumen, all crafting masters | Advanced recipes, crafting speed bonus, commission board priority |
| Regional factions (per zone) | Zone story NPCs | Zone cosmetics, lore entries, housing decorations (Post-MVP) |

Reputation tiers: Stranger → Known → Trusted → Honored → Exalted

### NPC Schedules (Post-MVP)

Key NPCs follow daily routines:
- Crafting masters work during day, visit taverns at night
- Wanderer Syl changes region every Monday
- Trade NPCs have "busy hours" with unique dialogue
- World Boss Herald only appears 30 minutes before spawn

## NPC Dialogue Design

### Principles
1. Short — No dialogue longer than 3 sentences unless the player asks for more
2. Voiced highlights — Key story moments voiced, routine dialogue text-only
3. Alignment-aware — Every line has up to 3 variants (one per alignment)
4. No exposition dumps — Lore delivered through ambient NPC conversations and optional "Ask about..." menus
5. Personality first — Every NPC should be identifiable by dialogue alone

### Dialogue Structure
- Greeting (alignment-reactive, 1 sentence)
- Service menu (functional)
- "Ask about..." menu (optional lore, 2-3 topics per NPC)
- Farewell (personality-driven, 1 sentence)

### Example: Lumen (Resonance Mapper)

**Radiant player, Depth 40:**
> "Your gems sing with order. I prefer a little chaos in my resonance, but I respect the craft. What do you need?"

**Corrupted player, Depth 40:**
> "Void-touched crystals are my favorite to cut. The way they resist the chisel... exhilarating. What are we making?"

**Unbound player, Depth 40:**
> "A balanced soul. Your gems must harmonize beautifully. Let me see what you're working with."

## MVP NPC List

Minimum NPCs for launch (maps to [[MVP Design]] scope — Calm Shores + Stormreach):

### Calm Shores (Port Haven)
| NPC | Category | Priority |
|-----|----------|----------|
| Captain Maren Voss | Story | Critical |
| Tova Ironhand | Crafting (Gem Shaping) | Critical |
| Lumen | Crafting (Resonance Mapping) | Critical |
| Harbormaster Finn | NPC Marketplace | Critical |
| The Broker | Commission Board | Critical |
| Sparring Master Holt | Combat (Practice) | Critical |
| Lightkeeper Asha | Alignment (Radiant) | Critical |
| Voidcaller Myx | Alignment (Corrupted) | Critical |
| Wanderer Syl | Alignment (Unbound) | Critical |
| Dungeon Guide Pip | Combat (Dungeon) | High |
| 10-15 Ambient NPCs | Ambient | Medium |

### Stormreach
| NPC | Category | Priority |
|-----|----------|----------|
| Admiral Kael Stormwright | Story | Critical |
| Arena Warden Zara | Combat (PvP) | Critical |
| Caravan Master Ola | Trade (Routes) | High |
| World Boss Herald | Combat (World Boss) | High |
| Dungeon Guide Pip (instance) | Combat (Dungeon) | High |
| 8-12 Ambient NPCs | Ambient | Medium |

Total MVP NPCs: ~15 named + ~25 ambient = ~40 NPCs

## NPC Text Presentation

### Name Plate Hierarchy
- Story NPCs: Gold-colored name with title, alignment badge
- Service NPCs: White name with role subtitle (e.g., "Tova Ironhand — Gem Shaping Master")
- Ambient NPCs: Gray name, no subtitle (name only on inspect)

### Alignment Text Cues
- Radiant-aligned NPCs: Warm gold text tint, clean dialogue, references to order and light
- Corrupted-aligned NPCs: Purple text tint, cryptic dialogue, references to power and void
- Unbound-aligned NPCs: Silver text tint, measured dialogue, references to balance and adaptation

---

See also: [[World Design]], [[Alignment System]], [[Social Hubs]], [[Economy Model]], [[Crafting System]], [[MVP Design]]
