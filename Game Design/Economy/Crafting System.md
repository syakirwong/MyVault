---
tags:
  - gdd
  - economy
  - crafting
---

# Crafting System

## Crafting Philosophy

Crafting is **controlled randomness with player agency**. Outcomes are influenced, not guaranteed. Players use catalysts and techniques to bias results, but the mutation-driven gem system means perfect control is impossible — and that's the point.

Crafting mirrors combat: plan, commit, adapt. A master crafter reads material properties the way a master fighter reads the combat log.

All players can learn all four disciplines. Specialization comes from mastery depth, not arbitrary locks. Without P2P trading, there is no reason to restrict disciplines — depth replaces scarcity.

---

## Crafting Disciplines

### Gem Shaping

The core discipline. Gem Shapers create base gems, upgrade rarity tiers, and set mutation direction.

| Service | Level Req | Inputs | Output |
|---------|-----------|--------|--------|
| Shape Common Gem | 1 | 10 Shards + domain material | Common gem (chosen domain) |
| Shape Uncommon Gem | 15 | 25 Shards + 3 Refined materials | Uncommon gem (chosen domain) |
| Shape Rare Gem | 30 | 50 Shards + 5 Pristine materials | Rare gem (chosen domain) |
| Shape Epic Gem | 40 | 100 Shards + 3 Mythic materials | Epic gem (chosen domain) |
| Shape Legendary Gem | 50 | 200 Shards + 10 Mythic + 5 Resonance Tokens | Legendary gem (chosen domain) |
| Upgrade Rarity | 20 | Gem + Shards + tier materials | Increase gem rarity by 1 tier (resets mutations) |
| Set Mutation Bias | 25 | Gem + catalyst + 50 gold | Bias next mutation toward a specific direction (not guaranteed) |

**Legendary gems are craft-exclusive.** They never drop from any content. This ensures permanent demand for Gem Shaping.

### Catalyst Brewing

Catalysts are consumables that influence gem stability, mutation, and resonance during combat and dungeons.

| Catalyst | Tier | Level Req | Inputs | Effect |
|----------|------|-----------|--------|--------|
| Stabilizer | Minor | 5 | 5 Shards | Reduce stability loss by 25% for 1 dungeon |
| Stabilizer | Standard | 20 | 15 Shards + Refined material | Reduce stability loss by 50% for 1 dungeon |
| Stabilizer | Major | 35 | 30 Shards + Pristine material | Reduce stability loss by 75% for 1 dungeon |
| Mutation Catalyst | Minor | 10 | 10 Shards + domain material | Nudge next mutation toward chosen domain affinity |
| Mutation Catalyst | Standard | 25 | 25 Shards + 2 Refined materials | Strongly bias next mutation direction |
| Mutation Catalyst | Major | 40 | 50 Shards + Pristine + Resonance Token | Force specific mutation type (consumed on use) |
| Resonance Catalyst | Standard | 30 | 30 Shards + 2 domain materials | +25% resonance trigger chance for 1 dungeon |
| Resonance Catalyst | Major | 45 | 60 Shards + Mythic material + 2 Resonance Tokens | Guarantee one resonance trigger during next dungeon |

**Rare catalysts** from high-end content have complex properties — element affinity, mutation bias, and stability effects combined into a single consumable. These are the crafting endgame.

### Resonance Mapping

Resonance Mappers create items that reveal, enhance, or interact with the resonance system.

| Service | Level Req | Inputs | Output |
|---------|-----------|--------|--------|
| Resonance Probe | 10 | 15 Shards + 2 domain materials | Consumable: reveals if two equipped gems have an undiscovered resonance |
| Resonance Amplifier | 25 | 30 Shards + Refined materials | Consumable: +50% resonance trigger chance for a specific pair for 3 fights |
| Resonance Fragment | 35 | 50 Shards + Pristine + domain material | Consumable: temporarily unlocks one undiscovered resonance for 1 fight (try before you find) |
| Resonance Lock | 45 | 80 Shards + Mythic material + 3 Resonance Tokens | Permanent: locks a discovered resonance to always trigger at base rate (no conditional requirements) |
| Resonance Journal Entry | 50 | 100 Shards + 5 Resonance Tokens | Permanent: publish a discovered resonance to the server-wide Resonance Journal with your name |

Resonance Mapping is the **knowledge economy** discipline. Its outputs are information and probability manipulation, not raw power.

### Salvaging

Salvagers recover value from broken, unwanted, or surplus gems and materials.

| Service | Level Req | Inputs | Output |
|---------|-----------|--------|--------|
| Basic Salvage | 1 | Any gem | Shards (scales with rarity: Common 5, Uncommon 12, Rare 25, Epic 50, Legendary 100) |
| Mutation Salvage | 20 | Mutated gem | Shards + mutation residue (used in Catalyst Brewing) |
| Fragment Recovery | 30 | Broken gem fragments | Partial shards + domain material |
| Essence Extraction | 40 | Epic+ gem | Domain essence (rare crafting material for Gem Shaping) |
| Legendary Reclamation | 50 | Broken Legendary fragments | Mythic material + 2 Resonance Tokens + Legendary shard |

Salvaging closes the resource loop. Every gem that breaks or gets dismantled feeds materials back into Gem Shaping and Catalyst Brewing. **Nothing is truly wasted.**

---

## Discipline Interdependence

```
           ┌──────────────┐
           │  GEM SHAPING  │ ← Creates gems
           └──────┬───────┘
                  │ gems feed into
    ┌─────────────┼─────────────┐
    ▼             ▼             ▼
┌────────┐  ┌──────────┐  ┌───────────┐
│CATALYST│  │RESONANCE │  │ SALVAGING │
│BREWING │  │ MAPPING  │  │           │
└────┬───┘  └────┬─────┘  └─────┬─────┘
     │           │              │
     │ catalysts │ resonance    │ shards +
     │ modify    │ knowledge    │ materials
     │ gems      │ enhances     │ fuel
     │           │ builds       │ everything
     └───────────┴──────────────┘
              ↓
         Back to GEM SHAPING
```

- Gem Shaping creates the assets
- Catalyst Brewing modifies how assets behave in combat
- Resonance Mapping unlocks and enhances the interaction layer
- Salvaging recycles everything back into raw materials
- All four feed each other

---

## Crafting Progression

- Crafting level **1-50** per discipline (all 4 unlockable)
- Crafting XP earned from **crafting** (not gathering)
- Higher tiers require higher-level materials AND crafting skill

| Level | Unlock |
|-------|--------|
| 1 | Basic crafting (Common outputs) |
| 10 | Uncommon outputs, Minor catalysts, Resonance Probes |
| 20 | Rarity upgrades, Mutation Salvage, Standard catalysts |
| 30 | Rare outputs, Fragment Recovery, Resonance Amplifiers, material processing (Raw → Refined) |
| 35 | Resonance Fragments |
| 40 | Epic outputs, Essence Extraction, Major catalysts, material processing (Refined → Pristine) |
| 45 | Resonance Locks, rare catalyst recipes |
| 50 | Legendary outputs, Legendary Reclamation, Resonance Journal publishing, Crafter Signature |

---

## Material Tiers

| Tier | Source | Used For | Scarcity |
|------|--------|----------|----------|
| Raw | World gathering, basic mob drops | Common crafts | Abundant |
| Refined | Processing Raw, dungeon drops | Uncommon/Rare crafts | Moderate |
| Pristine | Challenge dungeons, world bosses | Rare/Epic crafts | Scarce |
| Mythic | High-risk dungeons, legendary quests | Epic/Legendary crafts | Very scarce |
| Domain Materials | Domain Extract service, specific dungeon ecology | Domain-specific crafting | Variable by domain |
| Mutation Residue | Mutation Salvage | Catalyst Brewing | Scales with mutation activity |

---

## Crafter Signature System

At crafting level 50 in any discipline, a crafter can apply their **Crafter Signature** to items.

- Signed gems display crafter name in the gem's provenance record permanently
- Crafter reputation tracked on public leaderboard (gems crafted, gems in circulation, commission ratings)
- Master crafters (reputation 100+) can post NPC commissions at premium rates
- Signature gems carry a market premium on the NPC marketplace (5-15% appraisal bonus)
- Master crafter title appears on build card and name plate

The signature system turns crafters into **recognizable figures** in the game economy. In a text-based game, a crafter's name on a Legendary gem is the ultimate prestige marker.

---

## NPC Commission Integration

Crafters offer services through the NPC Commission Board (see [[Trade System]]).

- Post commission offers (service type, materials required, fee)
- Buyers browse by discipline, crafter reputation, price
- NPC escrow protects both parties
- 5% NPC commission on fees
- Ratings build long-term reputation

This preserves the **merchant/crafter playstyle** without P2P trading. A player can spend their entire session crafting, listing commissions, and building reputation — the NPC system enables this loop.

---

## Economic Role

Crafting is **structurally essential** to the economy:

- **Legendary gems are craft-exclusive** — permanent demand for Gem Shaping
- **Catalysts are consumable** — continuous demand for Catalyst Brewing
- **Resonance knowledge is gated** — Resonance Mapping creates information value
- **Broken gems feed Salvaging** — gem permadeath creates Salvaging demand
- **Crafting fees are a primary gold sink** — if crafting participation drops, the deflation model breaks
- **All players can craft** — no artificial scarcity, but mastery depth creates quality differentiation

---

## Related Documents

- [[Economy Model]]
- [[Trade System]]
- [[Gem System]]
- [[Gem Identity System]]
