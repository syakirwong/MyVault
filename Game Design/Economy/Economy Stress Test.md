---
tags:
  - gdd
  - economy
  - stress-test
  - math
---

# Economy Stress Test

Models failure scenarios for the NPC-mediated economy. The NPC-only architecture eliminates several traditional MMO threats (RMT, scam trades, market cornering) but introduces new considerations around NPC pricing, gem breakage rates, and crafting balance.

---

## Stress Test Scenarios

### Scenario 1: Bot Farming

**Threat:** Automated accounts farm gold and shards continuously, inflating supply.

**Key Difference from P2P Economy:** Bots cannot sell to other players. They can only sell to NPCs and salvage gems. This significantly reduces bot profitability — there is no arbitrage opportunity.

**Assumptions:**
- Bots run High-Risk Dungeons for maximum yield (300-450 gold/hr per Economy Model)
- Effective optimized rate: ~350 gold/hr accounting for skill-based bonus failures
- Bots operate 24/7 (720 hours/month)
- Diminishing returns kick in after 3rd run/day per dungeon

**Impact:**
- Single bot (uncapped): 350 x 720 = **252,000 gold/bot/month**
- 100 bots: 25,200,000 gold/month
- At 10,000 concurrent players generating ~4,200,000 gold/day (126M/month), 100 bots add ~20% to monthly gold supply

**Mitigation:**
- **Diminishing returns:** Same dungeon gold drops reduce by 50% after 3rd run/day (hard cap at 5)
- **No P2P transfer:** Bots cannot monetize through RMT — gold is stuck on the bot account
- **Behavioral detection:** Pattern analysis for >16 hrs/day, zero chat, repetitive actions
- **Gem binding:** Dungeon-dropped gems require crafting conversion (Gem Shaping) to list on NPC marketplace — bots need crafting levels
- **Account value cap:** Without P2P trade, bot accounts have no external monetization path

**With mitigations:**
- Diminishing returns cap effective gold to ~175 gold/hr average
- Bot output reduced to ~126,000 gold/bot/month (~50% reduction)
- 100 bots with mitigations: ~12,600,000 gold/month (~10% of total supply)
- **No RMT path eliminates the primary incentive to run bots**

**Residual Risk:** Low — bots have no way to extract value from the game. They inflate NPC gold supply slightly but cannot transfer wealth to real players.

---

### Scenario 2: Duplication Exploits

**Threat:** Gem or gold duplication through technical exploits.

**Impact:** Potentially catastrophic — unique mutated gems duplicated would destroy provenance integrity.

**Mitigation:**
- **Server-side validation** on every transaction, craft, and mutation
- **Unique gem IDs** with creation timestamps and immutable lineage
- **Hourly supply audits** comparing total supply vs expected generation
- **Transaction atomicity** — all operations are ACID-compliant
- **Mutation hash** — each gem's mutation state generates a cryptographic hash; duplicates produce identical hashes, triggering alerts

**Detection:**
- Alert if any gem ID appears twice in the system
- Alert if any item type's supply increases by >2 sigma from expected daily generation
- Alert if any account's net worth increases by >300% in 24 hours

**Residual Risk:** Low — standard but critical engineering. Unique gem provenance makes duplicates trivially detectable.

---

### Scenario 3: NPC Price Manipulation

**Threat:** Players exploit the dynamic NPC pricing algorithm by listing and delisting gems to shift prices.

**Key Difference:** This replaces the traditional "market cornering" threat. Since NPCs set prices, manipulation targets the pricing algorithm, not other players.

**Attack Vector:**
- Player lists 50 Common Kinetics gems at maximum price (+15% of NPC midpoint per Trade System) to inflate the NPC's weighted average
- Delists after price shifts upward
- Sells actual gems at inflated NPC price

**Mitigation:**
- **Delist penalty:** 5% of listed price charged on delist (makes wash-listing expensive)
- **Weighted moving average:** NPC pricing uses 30-day volume-weighted average, resistant to short-term spikes
- **Outlier filtering:** Listings beyond the NPC's +/-15% adjustment range are excluded from average calculation
- **Listing caps:** 15 active listings per player (25 for master crafters) prevents bulk manipulation
- **Purchase history weighting:** Actual completed transactions weighted 3x higher than listings in price calculation
- **48-hour listing duration** with single renewal — limits sustained manipulation attempts

**Residual Risk:** Very Low — the NPC pricing algorithm is designed to be manipulation-resistant. The cost of attempted manipulation (delist fees) exceeds the potential gain.

---

### Scenario 4: Deflationary Spiral

**Threat:** Gem breakage + NPC spread fees remove too much value from the economy. Currency becomes scarce, players feel poor, engagement drops.

**This is the most important scenario for an NPC-only economy with gem permadeath.**

**Assumptions:**
- 10,000 concurrent players
- Average active play: 2 hours/day
- Activity mix: 50% dungeons, 25% open world, 15% crafting/town, 10% PvP
- Base gold generation: 200 gold/hr (blended across activity types per Economy Model source rates)
- Skill-based bonus rewards: ~5% average population uplift (some players earn +15-20%, most earn +0%)
- Effective blended rate: **210 gold/hr**

**Daily Gold Generation:**
```
10,000 players x 2 hrs/day x 210 gold/hr = 4,200,000 gold/day
```

**Daily Sinks Required:** ~4,280,000 gold/day (target 2% deflation)

**Sink Breakdown:**

| Sink | Calculation | Daily Gold Removed |
|------|------------|-------------------|
| NPC Marketplace spread | 25% of players trade, avg 120 gold spread (tiered 12-20% per Trade System) | 300,000 |
| Crafting fees | 30% of players craft, avg 200 gold per session | 600,000 |
| Stability insurance | 6,000 insured runs (60% of est. 10,000 daily runs) x 200 gold avg | 1,200,000 |
| Gem storage | 20% of players pay storage, avg 100 gold/day | 200,000 |
| Fast travel | 10,000 x 3 trips x 20 gold | 600,000 |
| Gem reforging | 5% of players reforge daily, avg 500 gold | 250,000 |
| Mutation catalyst use | 15% of players use catalysts, avg 100 gold per application | 150,000 |
| Shard conversion (gold to shards) | 10% of players, avg 100 gold (at 100 gold = 10 shards rate) | 100,000 |
| Guild upkeep | 500 guilds x 2,500 avg / 7 days | ~179,000 |
| NPC commission fees | 5% of crafter fees, est. 1,000 commissions/day x avg 50 gold fee | 50,000 |
| Prestige purchases | 2% of players, avg 1,000 gold | 200,000 |
| Miscellaneous (appraisal reports, domain extracts) | Est. 5% of players, avg 100 gold | 50,000 |
| **Total Sinks** | | **~3,879,000** |

**Gap Analysis:**
```
Daily sources:  4,200,000 gold
Daily sinks:    3,879,000 gold
Net:           +321,000 gold/day (inflationary: ~7.6%)
```

**Problem:** Sinks are insufficient. The NPC tiered spread fee (12-20% per Trade System) generates less sink than the Economy Model's stated 15-25% range because lower-value trades dominate volume. Additionally, the NPC-only model drives lower marketplace volume than a P2P auction house (players salvage more, trade less).

**Required Adjustments:**
1. **Increase insurance baseline** — scale to 250-300 gold average across dungeon tiers (adds ~300,000)
2. **Introduce gem maintenance** — periodic NPC "tuning" fee to keep evolved gems stable (100 gold/gem/week for gems with locked mutations, adds ~200,000)

**Adjusted Sink Total:** ~4,379,000 gold/day

**Revised Gap:**
```
Daily sources:  4,200,000 gold
Daily sinks:    4,379,000 gold
Net:           -179,000 gold/day (deflation: ~4.3%)
```

**Overshoot:** 4.3% deflation exceeds the 2% target. This is preferable to inflation but needs tuning.

**Fine-Tuning Levers:**
- Reduce gem maintenance fee to 75 gold/gem/week (reduces sink by ~50,000)
- Reduce insurance baseline to 225 gold average (reduces sink by ~150,000)
- At 225 gold insurance + 75 gold maintenance: net ~-29,000 gold/day (~0.7% deflation)
- Bridge remaining 1.3% with prestige NPC purchases: if 3% of players spend avg 200 gold/day on cosmetic titles, build card customization, and gem display effects, that adds ~60,000 to sinks

**Tuned Target:**
```
Daily sources:   4,200,000 gold
Daily sinks:    ~4,289,000 gold
Net:            ~-89,000 gold/day (~2.1% deflation)
```

**Emergency Levers:**
- Increase NPC spread fee by 2-3% across tiers
- Introduce limited-time NPC sink events (rare cosmetics, seasonal catalysts)
- Increase gem maintenance fee for Legendary gems specifically
- Reduce gold generation rates by 5-10% (least preferred)

**Sensitivity Analysis:**

| Variable | Change | Impact |
|----------|--------|--------|
| Insurance rate higher (75% of runs) | +300,000 sink | Overshoots deflation — reduce insurance cost to compensate |
| Active play 3 hrs | +2,100,000 source | Sinks must scale — insurance, crafting, and catalyst usage increase proportionally |
| Skill bonuses higher (10% avg) | +200,000 source | Marginal — absorbed by existing sink headroom |
| Gem breakage rate higher | More salvaging, more shards, less gold | Shifts economy toward shard-denominated |
| Player count 20,000 | Scales linearly | Ratios hold |
| NPC spread fee aligned to Economy Model (15-25%) | +100,000-200,000 sink | Helps close gap without new mechanisms |

---

### Scenario 5: Gem Breakage Rate Imbalance

**Threat:** Gem breakage in high-risk dungeons is too frequent (players feel punished) or too rare (no risk, no economic sink).

**Impact:**
- Too frequent: Players avoid high-risk content, Salvaging becomes oversupplied, gem values crash
- Too rare: No meaningful risk, gem supply inflates, economy stagnates

**Targets:**
- 5-15% of equipped gems break per high-risk dungeon run (per Economy Model health metrics)
- With 4 gems x 5 players = 20 gems at risk per group, 1-3 gems should break per run on average
- With insurance (Stabilizer catalysts from Catalyst Brewing): 0-1 gems break

**Mitigation:**
- **Insurance tiers** allow players to choose their risk level (Minor/Standard/Major Stabilizers reduce loss by 25%/50%/75%)
- **Stability drain is telegraphed** — players can see stability dropping and make tactical decisions
- **Overcharge is voluntary** — players opt into additional stability risk
- **Salvaging recovers partial value** — breakage is painful but not total loss (Common 5, Uncommon 12, Rare 25, Epic 50, Legendary 100 shards)
- **Legendary gems have higher base stability** — they're harder to break accidentally

**Monitoring:**
- Track breakage rate per dungeon per week (target: 5-15%)
- Track player sentiment surveys on "fairness of gem loss"
- Track Salvaging output vs Gem Shaping input (should be roughly balanced)

**Residual Risk:** Medium — requires careful tuning. Too much breakage will drive players away; too little will stagnate the economy.

---

### Scenario 6: New Player Experience

**Threat:** New players feel poor and locked out of meaningful content.

**Key Difference from P2P:** No market to be priced out of. New players interact with NPCs at fixed prices. Veteran wealth cannot inflate new player costs.

**Onboarding:**
- Tutorial rewards: 1 Common gem per domain (8 total, try everything)
- NPC vendor sells all Common gems at 100 gold each (price anchor per Economy Model)
- Quest rewards provide materials for first crafting projects
- First 30 days: reduced NPC spread fee (15% -> 10%) on marketplace
- Starter catalyst pack (3 Minor Stabilizers, 2 Minor Mutation Catalysts)

**Progression Viability:**
- After 10 hours of play: ~2,100 gold (at 210 gold/hr blended) + ~50 shards
- Cost for 4 Common gems + basic catalysts: ~600 gold + 20 shards (per Economy Model price anchors)
- **New player is fully operational within 4-5 hours** with quest gem rewards
- Marketplace browsing available immediately — can buy Uncommon gems after ~15 hours
- Shard conversion available at 100 gold = 10 shards if shard-short

**Residual Risk:** Very Low — NPC pricing eliminates the veteran-inflation problem entirely. Fixed price anchors ensure new players always have purchasing power.

---

### Scenario 7: Crafting Devaluation

**Threat:** Dungeon drops outclass crafted items, making crafting unprofitable.

**Mitigation:**
- **Legendary gems are craft-exclusive** — permanent demand (per Crafting System: Shape Legendary requires Lv 50 + 200 Shards + 10 Mythic + 5 Resonance Tokens)
- **Catalysts are consumable** — continuous demand (every dungeon run consumes catalysts for stability and mutation control)
- **Dungeon-dropped gems require Gem Shaping conversion** to list on marketplace
- **Crafted gems have guaranteed domain** vs random domain on drops
- **NPC commissions provide steady income** for master crafters (5% NPC fee is the cost of the escrow service)
- **Crafter Signatures add premium value** (5-15% appraisal bonus per Trade System formula)

**Residual Risk:** Low — crafting is structurally essential. Consumable catalysts alone guarantee permanent demand.

---

### Scenario 8: Shard Economy Collapse

**Threat:** Shards become either worthless (oversupplied from excessive salvaging) or impossibly scarce (not enough gems breaking).

**Shard Sources (per Economy Model):**
- Salvaging broken/unwanted gems (primary: 5-100 shards per gem by rarity)
- Minor combat drops (3-12 shards/hr depending on content)
- Gold conversion at NPC (100 gold = 10 shards)

**Shard Sinks (per Crafting System):**
- Gem Shaping (10-200 shards per craft)
- Catalyst Brewing (5-60 shards per catalyst)
- Resonance Mapping (15-100 shards per item)
- Gem reforging (30-50 shards per reforge)

**Monitoring:**
- Track shard supply/demand ratio (target: 0.8-1.2 per Economy Model)
- Track shard supply growth rate (target: stable +/-5% per month)
- Track shard sources: salvaging vs combat drops vs gold conversion
- Track shard sinks: crafting consumption vs catalyst brewing vs resonance mapping

**Balancing Levers:**
- Adjust salvaging yields per rarity tier
- Adjust crafting shard costs
- Adjust gold-to-shard conversion rate at NPC
- Introduce shard-only prestige purchases if oversupplied
- Increase combat shard drops if undersupplied

**Residual Risk:** Medium — the shard economy is tied to gem breakage and salvaging rates, which are harder to predict than gold flows. Requires active monitoring.

---

### Scenario 9: Resonance Token Scarcity

**Threat:** Resonance Tokens are too scarce, gating Legendary crafting and high-end Resonance Mapping behind excessive grind. Or too abundant, trivializing endgame progression.

**Sources (per Economy Model):**
- Dungeon boss kills (High-Risk only): 1-2 per run
- Resonance discoveries: variable
- High-risk dungeon completions: guaranteed 1

**Sinks (per Crafting System):**
- Shape Legendary Gem: 5 Resonance Tokens
- Major Mutation Catalyst: 1 Resonance Token
- Major Resonance Catalyst: 2 Resonance Tokens
- Resonance Lock: 3 Resonance Tokens
- Resonance Journal Entry: 5 Resonance Tokens
- Legendary Reclamation output: returns 2 Resonance Tokens (partial recycling)

**Scarcity Model:**
- A dedicated endgame player doing 2 high-risk runs/day earns ~3-4 tokens/day
- Crafting one Legendary gem costs 5 tokens (~1.5 days of focused farming)
- This feels right — Legendaries should take effort but not be unreachable

**Failure Modes:**
- **Too scarce:** Players hoard tokens, never craft Legendaries, endgame feels locked. Master crafters can't fulfill commissions.
- **Too abundant:** Legendary gems flood the market, lose prestige value. Resonance Locks become trivial.

**Balancing Levers:**
- Adjust boss token drop rates
- Add token rewards to seasonal content or achievement milestones
- Adjust Legendary crafting cost
- Legendary Reclamation returns tokens — breakage partially recycles them

**Residual Risk:** Medium — Resonance Tokens gate the most aspirational content. Getting the earn rate wrong impacts endgame retention significantly.

---

## Economic Health Dashboard

| Metric | Healthy | Warning | Critical |
|--------|---------|---------|----------|
| Gold supply growth/month | -2% to +2% | +3% to +5% | > +5% |
| Shard supply growth/month | -5% to +5% | +/-5-10% | > +/-10% |
| Shard supply/demand ratio | 0.8-1.2 | 0.5-0.8 or 1.2-1.5 | < 0.5 or > 1.5 |
| Gem breakage rate (high-risk) | 5-15% | 3-5% or 15-25% | < 3% or > 25% |
| NPC marketplace volume/day | > 30,000 | 15,000-30,000 | < 15,000 |
| Common gem NPC price | 80-120 gold | 60-80 or 120-160 | < 60 or > 160 |
| Rare gem NPC price | 800-1,500 gold | 500-800 or 1,500-2,500 | < 500 or > 2,500 |
| Insurance usage | 30-60% of runs | < 20% or > 80% | < 10% or > 90% |
| Crafting profitability | > 15% margin | 5-15% | < 5% |
| New player 30-day retention | > 40% | 30-40% | < 30% |
| Gini coefficient (wealth) | < 0.55 | 0.55-0.70 | > 0.70 |
| Bot detection rate | > 95% within 48hrs | 80-95% | < 80% |
| Commission board usage | > 10% of crafters | 5-10% | < 5% |
| Resonance Token earn rate | 3-4/day (active endgame) | < 2 or > 6 | < 1 or > 10 |

---

## Quarterly Review Checklist

- [ ] Review gold supply growth against target (-2% to +2%)
- [ ] Review shard supply/demand ratio against target (0.8-1.2)
- [ ] Review shard supply growth against target (-5% to +5%)
- [ ] Analyze gem breakage rates per dungeon tier
- [ ] Check NPC marketplace price trends across domains and rarities
- [ ] Verify NPC spread fee tiers (12-20%) are generating expected sink volume
- [ ] Verify crafting remains profitable (margins > 15%)
- [ ] Survey new player purchasing power (fully operational within 5 hours?)
- [ ] Audit for suspicious account patterns (bot behavior, anomalous wealth)
- [ ] Review insurance pricing (usage 30-60% sweet spot)
- [ ] Track shard source/sink balance across all four crafting disciplines
- [ ] Track Resonance Token earn/spend rates (3-4/day target for active endgame players)
- [ ] Model impact of upcoming content on economy
- [ ] Review gem maintenance fee adequacy (if implemented)
- [ ] Check commission board health (crafter participation, buyer satisfaction, NPC fee revenue)
- [ ] Verify price anchors hold (Common gem 100g, Basic catalyst 50g, Short travel 10g)

---

## Cross-Document Alignment Notes

The following values are sourced from and must stay consistent with the Economy Model and Trade System:

| Parameter | Source Document | Value |
|-----------|----------------|-------|
| Gold generation rates | Economy Model: Source Rates | 80-450 gold/hr by activity |
| Skill-based bonuses | Economy Model: Skill-Based Bonus Rewards | +5% to +20% per modifier |
| NPC spread fee tiers | Trade System: NPC Spread Fee | 12-20% tiered by value |
| Delist penalty | Trade System: Listing Rules | 5% of listed price |
| Listing caps | Trade System: Listing Rules | 15 per player, 25 for master crafters |
| Price adjustment range | Trade System: Listing Process | +/-15% of NPC midpoint |
| NPC commission fee | Trade System: Commission Board | 5% of crafter fees |
| Shard conversion rate | Economy Model: Price Anchors | 100 gold = 10 shards |
| Insurance range | Economy Model: Sink Rates | 100-2,000 gold |
| Salvage yields | Crafting System: Salvaging | 5-100 shards by rarity |

> [!note] Spread Fee Discrepancy
> The Economy Model states NPC spread at "15-25% of gem value" while the Trade System specifies tiered rates of 12-20%. The Trade System's tiered structure is the authoritative specification. The Economy Model should be updated to reflect the 12-20% tiered rates, or the tiers should be adjusted upward to match the Economy Model's intended sink pressure. This stress test models using the Trade System's 12-20% rates.

---

## Related Documents

- [[Economy Model]]
- [[Crafting System]]
- [[Trade System]]
