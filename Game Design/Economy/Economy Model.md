---
tags:
  - gdd
  - economy
  - model
---

# Economy Model

## Economic Philosophy

The economy is an **NPC-mediated sink-source model**. There is no direct player-to-player trading. All transactions flow through NPC intermediaries who act as trusted brokers, appraisers, and regulators.

This eliminates RMT (Real Money Trading) by design. Players never touch each other's assets directly. The NPC layer provides provenance tracking, price stability, and fraud prevention as structural guarantees, not bolt-on security.

The economy rests on four pillars:

1. **NPC intermediation** — all trades flow through NPCs, eliminating P2P exploit vectors
2. **Controlled generation** — predictable, bounded currency and gem inflows
3. **Mandatory sinks** — unavoidable costs tied to core gameplay loops
4. **Appraisal-based pricing** — every gem has a unique value based on domain, rarity, mutation history, and resonance mastery

Target: **slight deflation** — sinks exceed sources by ~2%. Gems break, currency is scarce, saving is rewarded.

---

## Currencies

### Gold (Primary)

| Property | Detail |
|----------|--------|
| Source | Dungeon clears, quest rewards, NPC gem sales, salvage proceeds |
| Sink | NPC spread fees, crafting costs, stability insurance, gem storage, reforging, fast travel |
| Tradeable | N/A (no P2P) |
| Target Trend | Slight deflation (sinks > sources by ~2%) |

Gold is the universal medium of exchange. Every NPC transaction is denominated in gold.

### Shards (Secondary / Crafting)

| Property | Detail |
|----------|--------|
| Source | Salvaging broken/unwanted gems, minor combat drops, crafting from raw materials |
| Sink | Catalyst crafting, resonance mapping, gem reforging |
| Tradeable | N/A (no P2P) |
| Convertible | Can purchase with gold at NPC (gold sink) |

Shards are the crafting fuel. They represent raw gem essence recovered from broken or dismantled gems. Every gem that breaks or gets salvaged feeds back into the crafting economy.

### Resonance Tokens (Tertiary / Prestige)

| Property | Detail |
|----------|--------|
| Source | Dungeon boss kills, resonance discoveries, high-risk dungeon completions |
| Sink | Legendary catalysts, exclusive gem variants, resonance research acceleration |
| Tradeable | N/A (no P2P) |

Resonance Tokens gate access to the highest tier of crafting and mutation content. They cannot be farmed passively — they require active engagement with the hardest content.

### Honor (PvP)

| Property | Detail |
|----------|--------|
| Source | PvP wins, seasonal rankings, momentum mastery milestones |
| Sink | PvP cosmetics, wager match entry, prestige titles |
| Tradeable | N/A |

Honor rewards competitive engagement without distorting the gem economy.

---

## Source Rates (per hour of active play)

| Activity | Gold/hr | Shards/hr | Resonance Tokens | Notes |
|----------|---------|-----------|------------------|-------|
| Dungeon (Normal) | 150-200 | 3-5 | 0 | Consistent baseline |
| Dungeon (Challenge) | 200-300 | 5-8 | 0-1 | Higher skill = higher reward |
| High-Risk Dungeon | 300-450 | 8-12 | 1-2 | Gem breakage possible |
| Open World | 80-120 | 5-8 | 0 | Material/shard focused |
| World Boss | 50-100 (per kill) | 5-10 | 1 | Time-gated |
| PvP Arena | 50-100 | 0 | 0 | Honor primary reward |
| Crafting (NPC sales) | Variable | -shards | 0 | Economy-driven |
| Salvaging | 20-50 | 5-15 | 0 | Gem quality determines yield |

**Design intent:** Dungeons are the gold generation baseline. High-risk dungeons offer the best rates but risk gem breakage. Salvaging is the primary shard source, creating a recycling loop.

### Skill-Based Bonus Rewards

Combat performance modifies dungeon gold rewards:

| Performance | Gold Modifier |
|-------------|--------------|
| Momentum averaged 7+ across all rounds | +15% gold |
| Resonance triggered during boss fight | +10% gold per resonance |
| Zero gem stability loss (clean run) | +20% gold |
| Rounds survived beyond expected | +5% per extra round |

---

## Sink Rates

| Sink | Cost | Frequency | Notes |
|------|------|-----------|-------|
| NPC Marketplace spread | 12-20% of gem value (tiered) | Per sale | Primary gold sink. See [[Trade System]] for tier breakdown |
| Crafting fees | 50-500 gold + shards | Per craft | Tier-based |
| Stability insurance | 100-2,000 gold | Per dungeon entry | Scales with gem rarity and dungeon risk |
| Gem storage (beyond 20 slots) | 50 gold/slot/week | Weekly | Encourages selling/salvaging excess |
| Gem reforging | 200-5,000 gold + shards | Per reforge | Domain re-roll or mutation direction |
| Fast travel | 10-30 gold | Per use | Distance-based |
| Mutation catalyst use | 50-300 gold + shards | Per catalyst | Voluntary stability sacrifice |
| Prestige purchases | 500-10,000 gold | Optional | Cosmetic titles, build card borders |
| Guild upkeep | 1,000-5,000 gold/week | Weekly | Guild size-based |

**Design intent:** NPC marketplace spread is the primary sink at scale. Insurance and storage fees create recurring costs tied to core loops. Prestige purchases serve as wealth sinks for veterans.

---

## Price Anchors

Stable-price items that anchor the economy:

| Item | NPC Price | Function |
|------|-----------|----------|
| Common gem (any domain) | 100 gold | Floor reference for gem value |
| Basic catalyst (Minor tier) | 50 gold | Floor for crafting consumables |
| Stability insurance (Normal dungeon) | 100 gold | Minimum risk mitigation cost |
| Fast travel (short distance) | 10 gold | Minimum convenience cost |
| Shard conversion (100 gold → 10 shards) | 100 gold | Links gold and shard economies |

These anchors give gold a minimum purchasing power. The NPC marketplace adjusts dynamically around these anchors but never drops below them.

---

## NPC Gem Marketplace

See [[Trade System]] for full specification.

The marketplace replaces the traditional Auction House. Players list gems with NPCs, NPCs appraise and price them, other players browse and buy anonymously. No direct player contact. Full provenance tracking.

---

## Economic Health Metrics

| Metric | Healthy Range | Warning | Critical |
|--------|--------------|---------|----------|
| Gold supply growth/month | -2% to +2% | +3% to +5% | > +5% |
| NPC marketplace daily volume | > 30,000 transactions | 15,000-30,000 | < 15,000 |
| Median Common gem NPC price | 80-120 gold | 60-80 or 120-160 | < 60 or > 160 |
| Median Rare gem NPC price | 800-1,500 gold | 500-800 or 1,500-2,500 | < 500 or > 2,500 |
| Shard supply/demand ratio | 0.8-1.2 | 0.5-0.8 or 1.2-1.5 | < 0.5 or > 1.5 |
| Insurance usage rate | 30-60% of dungeon runs | < 20% or > 80% | < 10% (too expensive) or > 90% (too cheap) |
| New player 30-day retention | > 40% | 30-40% | < 30% |
| Gini coefficient (wealth) | < 0.55 | 0.55-0.70 | > 0.70 |
| Crafting profitability | > 15% margin | 5-15% | < 5% |
| Gem breakage rate (high-risk) | 5-15% of equipped gems | < 3% or > 25% | < 1% or > 40% |

---

## Related Documents

- [[Economy Stress Test]]
- [[Crafting System]]
- [[Trade System]]
