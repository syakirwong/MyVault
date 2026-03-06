---
tags:
  - gdd
  - mvp
  - production
---

# MVP Design

The minimum viable product to validate the core gameplay loop: **Build experimentation through gems in a reactive text-based interface.**

---

## MVP Includes

### Reactive UI Framework

- Combat panel with action bar, enemy intent, combat log
- Hub panel with context-sensitive actions
- Build panel with gem loadout and build codes
- Text color language for elements, alignment, and rarity
- See [[UI Design]] for full specification

### Gem Domains (8)

- Kinetics, Entropy, Resonance, Void, Flux, Tempest, Aegis, Synthesis
- 4 equipped per player, 70 possible loadouts
- Each domain: 3-4 base actions, systemic rule, mutation tendency
- See [[Gem Identity System]] for full domain specifications

### Resonance System

- ~80 resonances across 28 domain pairs at MVP
- Passive, Active, and Conditional tiers
- Discovery-driven — not documented in-game

### Phase-Based Combat

- Draw → Commit → Clash → Aftermath (4 phases per round)
- See [[Combat System]] for full specification

### Alignment System

- All 3 alignments functional
- Depth 0-40 for MVP (Initiate and Devoted tiers)
- Gem modification by alignment working
- Alignment badges and name plate coloring

### Content

- 2 regions: Calm Shores + Stormreach
- 3 dungeons: Tidecaller Caverns, Stormspire, one hybrid
- 1 world boss
- Main storyline for 2 regions (~10 hours)
- Arena PvP (1v1 and 3v3)

### Economy

- Gold + Shards + Resonance Tokens (3 currencies)
- NPC Gem Marketplace (appraisal-based, anonymous listings)
- NPC Commission Board (crafter services)
- 4 crafting disciplines: Gem Shaping, Catalyst Brewing, Resonance Mapping, Salvaging
- Stability insurance system
- 1 crafting hub

### Social

- Build publishing (basic)
- Build codes
- Player inspect (build card display)
- 1 social hub (Port Haven)

---

## MVP Excludes (Post-Launch)

- Additional gem domains (9+)
- Overload slot (5th gem)
- Legendary resonances
- Regions: Verdant Depths, Obsidian Wastes, Celestial Peaks
- Naval combat
- Raids
- Trade routes
- Creator leaderboards (full system)
- Guild halls
- Alignment depth beyond 40
- Advanced cosmetics (combat log styles, profile banners)
- Gem permadeath content

---

## MVP Success Metrics

| Metric | Target | Method |
|--------|--------|--------|
| Session length | 25-40 min average | Analytics |
| Build diversity | 30+ viable loadouts in PvP | Win rate analysis (no loadout >60% WR) |
| Gem experimentation | Avg player tries 5+ loadouts in first month | Loadout tracking |
| Resonance discovery | 50%+ of MVP resonances found by community in month 1 | Discovery tracking |
| Crafting participation | 30%+ of players craft regularly | Activity tracking |
| 30-day retention | 40%+ | Cohort analysis |
| Marketplace usage | 50%+ of players list at least 1 gem | NPC marketplace tracking |
| Economy stability | Gold supply growth < 3%/month | Economic monitoring |
| UI engagement | <2s avg response to enemy intent | Interaction analytics |

---

## MVP Development Priorities (ordered)

1. Phase-based combat engine (Draw → Commit → Clash → Aftermath)
2. 8 gem domains with action pools and systemic rules
3. Mutation and stability system
4. Resonance system (~80 MVP resonances)
5. Momentum system (0-10 scale, thresholds, momentum/desperation actions)
6. Reactive UI framework (combat panel, hub panel, build panel)
7. Emergent archetype naming
8. Alignment system + UI indicators
9. 2 regions + 3 dungeons (with gem ecology)
10. NPC Marketplace + Commission Board + Crafting (4 disciplines)
11. Arena PvP (1v1 and 3v3)
12. Build publishing + build cards
13. Balance + tuning

---

## Related Pages

- [[GDD Hub]]
- [[Gem Identity System]]
- [[Gem System]]
- [[Gem System Math]]
- [[UI Design]]
