---
tags:
  - research
  - pko
  - economy
source: https://piratekings.online/database/trade-simulator
captured: 2026-10-23
---

# PKO — Sea Trade Economy

Parsed from the portal's trade simulator, which lists each port trader's buy and sell tables. The analysis is mine.

## Structure

- **12 trading ports**, each with a Trader NPC: Argent City, Thundoria Harbor, Zephyr Isle, Glacier Isle, Outlaw Isle, Isle of Chill, Canary Isle, Cupid Isle, Shaitan City, Isle of Fortune, Lower Icicle Castle and Spring Town. Shaitan also has specialist suppliers for arms, rare materials and rare metals.
- **61 buy listings** cover 53 unique trade goods, and each good sells at about 4.2 destination ports on average. You can only sell a good at certain ports, so goods are route-specific, not universal.
- Prices are shown as a **±10% band** (for example 279…341). The real price floats inside it, probably driven by recent supply. *I couldn't verify the exact mechanism from public data.*
- Trade goods are bought at one port, carried by ship (see [[PKO - Economy & Crafting]] → Ship System) and sold at a port where the good is "foreign".

## Margin analysis (midpoint prices, 257 buy→sell pairs)

| Metric | Value |
|--------|-------|
| Median sell/buy ratio | **2.02×** |
| 10th percentile | 1.84× |
| 90th percentile | 2.20× |
| Best single run | +1,052 g/unit (Army Gunpowder, Shaitan → Thundoria) |
| Best round-trip loop | Thundoria ↔ Lower Icicle Castle, +1,624 g/unit |

**Top routes by profit per unit**

| From → To | Good | Buy | Sell | Profit |
|-----------|------|-----|------|--------|
| Shaitan → Thundoria | Army Gunpowder | 1,100 | 2,152 | +1,052 |
| Thundoria → Lower Icicle | High-Grade Battleship Component | 564 | 1,560 | +996 |
| Thundoria → Lower Icicle | Imitation Royal Tableware | 634 | 1,628 | +993 |
| Shaitan → Thundoria | Exceptional Apparatus | 950 | 1,860 | +910 |
| Outlaw → Lower Icicle | Pirate Eye Patch | 634 | 1,418 | +783 |
| Thundoria → Canary | Hamburger | 714 | 1,474 | +759 |

## Design read

- Margins are **deliberately flat** (nearly every route is about 2×). The designers tuned for *distance and risk*, not price discovery: profit per hour depends on sailing time, cargo capacity and sea-monster or PK risk, not on finding a hidden arbitrage.
- Because buying ties up gold, trade is a **gold faucet gated by capital**: richer players move more cargo. That's a rich-get-richer loop with no sink unless ships get lost or damaged.
- A static table means the meta gets "solved" fast. The community simulator publishes the solution, which kills discovery.

## Takeaways for [[Trade System]] and [[Economy Model]]

1. A text MMO can *be* this loop: route planning, risk and cargo choices map cleanly onto UI cards. Consider a "voyage" mechanic with event rolls.
2. Make prices **dynamic and player-driven**: selling floods a port and lowers its price, which recovers over time. That keeps routes from being solved and spreads players out.
3. Add a sink: tariffs, cargo insurance or loss risk. This matches the faucet and sink balance in [[Economy Stress Test]].
4. Keep a ±band display. It shows enough to plan with while leaving uncertainty.

---
Back to [[PKO Teardown]]
