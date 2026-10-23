---
tags:
  - research
  - pko
  - hub
source: https://pirateking.online/
captured: 2026-10-23
---

# PKO Teardown

A reverse-engineering pass on **Pirate King Online** (a Tales of Pirates private server, live since July 2016, run by ServerDev LLC), used as reference material for [[GDD Hub|Stormbound Online]].

**Method:** passive analysis of public pages only.
- Landing page and portal HTML.
- The 70 knowledge-center guides, paraphrased by topic.
- The game database, the simulator JS (formulas extracted), the sea-trade tables (parsed and analysed), the rank ladder (aggregated with no player names) and the mall.

No logins, no game client and no server probing.

## Notes

| Note | What's in it |
|------|--------------|
| [[PKO - Tech & Infrastructure]] | Netlify landing page plus the XenForo 2.3 portal, the custom database/simulator add-ons, client distribution, and the live class meta |
| [[PKO - Stat Formulas]] | **Exact** derived-stat formulas, per-class coefficients, the modifier stack and skill buffs, recovered from code |
| [[PKO - Core Systems]] | Lore, races, class tree, stats, skills, levelling, quests, Rebirth, PvP, mentorship, UI |
| [[PKO - Economy & Crafting]] | Gems (in depth), forging odds, sockets, apparel, life skills, production, stalls, bank, ships, Lv85 gear, faucets and sinks |
| [[PKO - Sea Trade Economy]] | 12 ports, 53 goods, margin and route analysis |
| [[PKO - Social, Endgame & Monetization]] | Guilds, fairy pets and breeding, gambling, loot boxes, dungeons and their schedules, Rum mall, P2W assessment |

## PKO at a glance

| Dimension | Finding |
|-----------|---------|
| Core loop | Grind mobs, sell drops to NPCs (the only real gold faucet), then sink the gold into the gem, forge and upgrade RNG chain |
| Build identity | 4 base classes become 6 advanced classes. One shared stat formula; classes differ only by coefficients. Convex (squared) stat curves push builds toward a single stat |
| Progression spikes | Lv10 class, Lv40 advanced class, Lv75 1st Rebirth, 2nd Rebirth, Lv90 cap. 65% of ranked players sit at the cap |
| Endgame | Scheduled portal mazes (3-hour cycle), a world boss on an 84-hour respawn, a survivor-split PvP arena, Lv85 unseal gear |
| Economy | One faucet against steep sinks. Sea trade is a flat ~2× arbitrage. **No tax on player trade** is the main hole |
| Monetization | "Rum" premium currency, tradeable 1:1 for gold. Pets and loot-box keys are the spend multipliers. P2W rated about 3.5/5 |
| Balance smell | Crusader makes up 40% of the top ladder. Its agility, flee and attack-speed coefficients are uniquely boosted, and attack speed has a hard cap |

## Lessons for Stormbound

> [!success] Applied to the GDD on 2026-10-23
> The Steal, Avoid and Adapt items below have been written into the relevant Stormbound notes. Search the vault for "Adapted from PKO" or "Avoid-list item" to find each change.

### Steal

1. **One formula, per-archetype coefficients.** This is easy to balance and easy to explain. Map it onto gem affinities; see [[PKO - Stat Formulas]] and [[Gem System Math]].
2. **Per-mille integer stat math** with a two-layer modifier stack (item layer, then skill layer). It's deterministic and works server-side. See [[Technical Architecture]].
3. **A public database and build simulator sharing the server's formula module.** PKO's community runs on these. For a build-experimentation game they're core features; see [[UI Design]] and [[Creator Ecosystem]].
4. **Player-set PvP rule toggles** for bar duels, and **guild immunity** in contested zones. See [[PvP Structure]] and [[Guild System]].
5. **Survivor-split rewards** (Chaos Argent): fewer survivors means a bigger share each. It creates emergent betrayal at no design cost.
6. **Scheduled portal windows** create appointment play and a social rhythm. See [[PvE Structure]].
7. **A use-it-or-lose-it weekly boost** (Pucca hourglass) is a free retention hook that doesn't stack with paid boosts. See [[Monetization]].
8. **Paid global chat** limits spam and drains gold. See [[Chat & Communication System]].

### Avoid

1. **Convex stat curves.** They collapse build diversity, which is the opposite of the [[Gem Identity System]] goal ("horizontal depth over vertical power").
2. **Compounding RNG destruction chains.** Reaching Lv5 in one socket expects about 195 Lv1 gems, which feels awful. Use pity timers or deterministic progress meters. See [[Gem System]] and [[Crafting System]].
3. **Untaxed player trade.** Add a market or stall fee. See [[Trade System]] and [[Economy Stress Test]].
4. **Monetized security** (paid item locks) and **unpublished loot-box odds.** See [[Account & Security System]] and [[Monetization]].
5. **Static trade tables.** Solved on day one. Use player-driven price elasticity.
6. **Farmable mentor rewards** that pay out regardless of the outcome. Tie them to the disciple's milestones. See [[Social & Friends System]].
7. **Hand-ported formula JS** that can drift from the server. Build from one source instead.

### Adapt (text-MMO translation)

- **Sea trade becomes voyage cards:** route, cargo and risk event rolls are a natural fit for a UI-driven game.
- **Fairy pets become a companion gem slot:** a slow passive growth meter and breeding as gem fusion, *without* the Rum-gated failure.
- **Rebirth becomes prestige with a small permanent gain** (PKO: ±5.5%) plus a full respec. That's a large perceived value for a small balance cost. See [[Progression Systems]].

## Open questions

- USD price per Rum, loot-box contents and odds, and whether Demonic Fruit drops anywhere. None of these are public.
- Whether trade prices really float inside the ±10% band, and what drives them.
- The "Sacred War" faction mode mentioned in the guides but never explained.
