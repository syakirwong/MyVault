# Stormbound Online — Vault Context

## Game Design Document

The GDD lives in `Game Design/` and is indexed from `Game Design/GDD Hub.md`.

### Structure

- **Core Systems** — Gameplay loops, progression, stats, alignment, factions, death/defeat
- **Combat** — Combat system, gem identity, skill/modifier gems, combat logs, bosses
- **Content** — PvE, PvP, world design, NPCs, onboarding, quests, lore, spectator mode
- **Economy** — Economy model, stress tests, loot, crafting, trade
- **UI & Identity** — Behavioral identity, UI design, cosmetics, accessibility, audio, settings
- **Social** — Creator ecosystem, social hubs, guilds, chat, friends, moderation
- **Meta & Live Ops** — Meta management, localization, patch/communication
- **Technical** — Architecture, account/security, state persistence, platform, analytics
- **Production** — MVP design, monetization, long-term vision

### Core Vision

Stormbound Online is a reactive text-based MMORPG where build experimentation is the primary gameplay driver. Players interact through dynamic UI elements rather than typing commands or navigating pixel worlds.

### Build Identity Balance

- Origin: 20% (starting stats and passive ability)
- Skill Choice: 30% (the canvas for gems)
- Gem Modifiers: 50% (the main creativity driver)

## Working Rules

- When editing GDD documents, preserve Obsidian wikilink syntax (`[[Page Name]]`)
- Maintain frontmatter tags on all GDD pages
- The implementation codebase is at `~/Documents/stormbound-online/`
