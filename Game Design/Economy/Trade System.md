---
tags:
  - gdd
  - economy
  - trade
---

# Trade System

## Design Principle

All trade flows through NPCs. Players never exchange items or currency directly. This eliminates RMT, scams, and market manipulation by design — not by enforcement.

The NPC acts as a trusted broker: appraising gems, setting prices, holding inventory in escrow, and completing transactions anonymously. Players influence supply by listing and buying. The NPC ensures fairness.

---

## NPC Gem Marketplace

### How It Works

```
LISTING FLOW:
Player A → Lists gem with NPC → NPC appraises → Sets price range
                                                → Player accepts or adjusts
                                                → Gem held in NPC escrow

BUYING FLOW:
Player B → Browses NPC listings → Sees appraised gems (anonymous seller)
                                → Purchases at listed price
                                → NPC transfers gold to Player A
                                → NPC takes spread fee
```

### Listing Process

1. Player brings gem to NPC Marketplace (available in all hub cities)
2. NPC **appraises** the gem based on:
   - Domain (Kinetics, Entropy, Void, etc.)
   - Rarity tier (Common through Legendary)
   - Mutation state (Stable, Shifting, Unstable, Critical — and specific mutations locked in)
   - Mastery level (how much the gem has been used)
   - Resonance history (which resonances this gem has participated in)
   - Crafter signature (if applicable)
3. NPC suggests a **price range** (floor to ceiling)
4. Player sets their price within the NPC's range (±15% of suggested midpoint)
5. Gem enters escrow — removed from player's inventory until sold or delisted

### Appraisal Pricing Formula

```
Base Value = Domain_Weight × Rarity_Multiplier

Mutation Bonus:
  +0% if Stable (no mutations locked in)
  +10-30% per locked-in mutation (based on mutation desirability)
  +50-100% for rare mutation combinations

Mastery Bonus:
  +2% per mastery level (max +20% at mastery 10)

Resonance Bonus:
  +5% per resonance this gem has triggered
  +25% for gems involved in Legendary resonance discovery

Crafter Signature:
  +5-15% for signed gems (scales with crafter reputation)

Market Factor:
  30-day volume-weighted average of completed sales ÷ static Base Value
  (completed sales weighted 3x vs listings), clamped to 0.80–1.20

Final Price = Base Value × (1 + Mutation + Mastery + Resonance + Signature) × Market Factor
```

**Market Factor** makes appraisal respond to player supply. If players flood the market with Kinetics Rares, those gems' price drops by up to 20% and recovers as supply normalizes. This keeps the market from being "solved" from a static table (a PKO avoid-list item; see [[PKO - Sea Trade Economy]]). The averaging and manipulation defenses are specified in [[Economy Stress Test]]. This term formalizes the 30-day weighted average that document already assumes.

### Buying Process

1. Player browses NPC marketplace by domain, rarity, mutation state, price range
2. All listings are **anonymous** — buyer does not know who listed the gem
3. Full **mutation history** is visible on each listing (verified by NPC, cannot be faked)
4. Optional: pay NPC for **detailed appraisal report** (50 gold) — shows stat breakdown, estimated DPS contribution, resonance compatibility with buyer's current loadout
5. Purchase completes instantly — NPC transfers gem to buyer, gold to seller (minus spread fee)

### NPC Spread Fee

| Transaction Value | Spread Fee | Gold Removed |
|-------------------|-----------|--------------|
| < 500 gold | 20% | Minimum sink on low-value trades |
| 500-2,000 gold | 18% | Standard trades |
| 2,000-10,000 gold | 15% | High-value trades |
| > 10,000 gold | 12% | Legendary trades — lower % but large absolute sink |

The spread fee is the primary gold sink, replacing the old AH tax. At scale, this removes more gold than any other mechanism.

### Listing Rules

| Rule | Detail |
|------|--------|
| Max active listings | 15 per player (25 for master crafters) |
| Listing duration | 48 hours (renewable once) |
| Expired listings | Gem returned to player, no fee refund |
| Delist penalty | 5% of listed price (prevents price fishing) |
| Price adjustment | Not allowed after listing — must delist and relist |

### Marketplace Categories

| Category | Contents |
|----------|---------|
| Gems — Base | Unmutated gems by domain and rarity |
| Gems — Evolved | Gems with locked-in mutations (unique items) |
| Catalysts | Mutation catalysts, stability catalysts, resonance catalysts |
| Materials | Raw, Refined, Pristine, Mythic crafting materials |
| Gear | Cosmetic/minor stat gear pieces |
| Fragments | Broken gem fragments for salvaging crafters |

---

## Gem Conversion

Players can convert unwanted gems instead of selling:

| Service | Cost | Result |
|---------|------|--------|
| **Salvage** | Free | Gem destroyed → Shards + fragments (amount scales with rarity/mutations) |
| **Reforge Domain** | 500-5,000 gold + 50 shards | Change gem's domain (keeps rarity, resets mutations) |
| **Reforge Mutation** | 200-2,000 gold + 30 shards | Reset one locked-in mutation to a random alternative |
| **Domain Extract** | 100 gold | Destroy gem → Domain-specific crafting material |

Conversion ensures no gem is truly "wasted." Even a Common Kinetics gem a player doesn't need produces shards, fragments, or domain materials.

---

## NPC Commission Board

Crafters can offer services to other players through NPC-mediated commissions.

### How It Works

1. **Crafter** posts a commission offer on the NPC board:
   - Service type (Gem Shaping, Catalyst Brewing, etc.)
   - Materials required (from the buyer)
   - Fee (gold, set by crafter)
   - Crafter signature and reputation displayed
2. **Buyer** browses commission board, selects a crafter
3. Buyer provides materials + fee to NPC escrow
4. NPC sends commission to crafter
5. Crafter completes the work, returns item to NPC
6. NPC delivers item to buyer, fee to crafter (minus 5% NPC commission)
7. Both parties rate the transaction

### Commission Rules

- Crafter never sees buyer's identity (and vice versa during transaction)
- Materials are held in NPC escrow — crafter cannot steal them
- Failed crafts (if applicable) return materials to buyer minus a small material loss
- Crafter reputation is public and persistent

This preserves the **social crafter identity** — master crafters build reputations and attract commissions — without enabling direct P2P asset transfer.

## Voyage Contracts (Post-MVP)

> [!info] Source
> Adapted from PKO's sea trade ([[PKO - Sea Trade Economy]]). PKO's version is a static ~2× arbitrage table that was solved on day one. Stormbound turns it into a decision loop built from cards.

Voyage Contracts are the trading layer on top of existing [[World Design#Travel System|sea travel]]. They're run by the Port Haven Trade Guild. Every step goes through NPCs; there is no player-to-player transfer ([[Technical Architecture#Constraints (Must Not Violate)]] #5).

### Loop

1. **Buy cargo** from a port's Trade Guild NPC. Each port sells 3-5 regional goods tied to its [[World Design#Gem Domain Ecology|domain ecology]].
2. **Choose a route and risk tier.** Calm, Contested or Storm-lane. Higher risk means more event cards and better destination prices.
3. **Sail.** The voyage plays as 3-5 **event cards** drawn from the route's deck: squall, pirate cutter, derelict, becalmed, rival trader, sea-beast. Each card offers 2-3 choices resolved with your build (combat with your normal gems, stat checks, or spending gold or cargo).
4. **Deliver** to a destination port NPC at that port's current price.

### Pricing

- Destination price = base value × route distance factor × **Port Demand**.
- **Port Demand** starts at 1.00 and falls 2% per cargo unit delivered server-wide in the last 24 hours (floor 0.70), then recovers over time. The best route moves around and players spread out, which is the dynamic-pricing fix from the avoid list.
- Prices are shown as a ±10% band before you sail. Enough to plan with, while leaving some uncertainty.

### Risk and Sinks

| Element | Effect |
|---|---|
| Cargo insurance | 5-15% of cargo value by risk tier. Covers 75% of cargo lost to event cards |
| Port tariff | 5% of sale value on delivery. A gold sink |
| Event losses | Failed event cards cost 10-40% of cargo |
| Contract cap | 3 Voyage Contracts per character per day. Keeps the faucet within the [[Economy Model]] deflation target |

### Rewards

- Net margin targets 1.3-1.6× cargo cost after tariff and insurance. Gold per hour is in line with Challenge dungeons (see [[Economy Model#Source Rates (per hour of active play)]]).
- Trade Guild reputation, voyage-log titles, and rare event cards that seed lore discoveries.
- Naval PvP ([[PvP Structure#Naval PvP (Post-MVP)]]) later layers guild-controlled routes over the Contested and Storm lanes.

---

## Price History & Transparency

All NPC marketplace transactions are logged and publicly visible.

- **Price charts:** 7-day, 30-day, 90-day views for every gem domain × rarity combination
- **Mutation premium tracking:** Average price premium for specific mutation types
- **Volume tracking:** Daily transaction volume per category
- **Price alerts:** Players set notifications for gem price thresholds
- **API access:** Public API for community tool builders (price trackers, build calculators)

Transparency keeps the NPC marketplace efficient. Players can make informed decisions about when to sell, salvage, or hold gems.

---

## Anti-Fraud Architecture

Since all trades flow through NPCs, most traditional MMO fraud vectors are eliminated by design:

| Traditional Threat | Status | Reason |
|-------------------|--------|--------|
| Scam trades | **Eliminated** | No direct P2P trading |
| RMT (gold selling) | **Eliminated** | No way to transfer gold between players |
| RMT (item selling) | **Eliminated** | No way to transfer items between players |
| Price manipulation | **Mitigated** | NPC sets price range; players can only adjust ±15% |
| Market cornering | **Mitigated** | NPC supply includes drops + crafting; no single buyer can control supply |
| Phishing/account theft | **Reduced** | Stolen accounts can't transfer assets out |

### Remaining Vectors

| Threat | Mitigation |
|--------|-----------|
| Account selling (sell whole account for real money) | 2FA required, device binding, suspicious login detection |
| Duplication exploits | Server-side validation, unique item IDs, hourly supply audits |
| NPC price manipulation (listing to shift dynamic prices) | Listing fees + delist penalties prevent wash trading; NPC pricing uses weighted moving average, resistant to spikes |

### Provenance Tracking

Every gem in the game has an immutable lineage record:

```
GEM: Kinetics Domain — Rare — Evolved
ID: GEM-KIN-R-0x7a3f29
─────────────────────────────────
CREATED: Crafted by [Ironforge Tova] (Gem Shaping Lv 42)
  Date: 2026-02-15
LISTED: NPC Marketplace, Port Haven
  Price: 1,200 gold
  Date: 2026-02-16
PURCHASED: [Anonymous]
  Date: 2026-02-17
MUTATIONS LOCKED IN:
  [1] Force Strike → Graviton Pulse (stability 68, locked 2026-03-01)
  [2] Systemic shift: displacement +15% (stability 51, locked 2026-03-10)
RESONANCES TRIGGERED: 3
  Kinetics+Tempest (Passive), Kinetics+Void (Active), Kinetics+Entropy (Conditional)
CURRENT OWNER: [Player]
STABILITY: 72/100
MASTERY: 7/10
```

This record is visible to anyone who inspects the gem (in marketplace listings, commission boards, or player build cards). It cannot be altered.

---

## Related Documents

- [[Economy Model]]
- [[Economy Stress Test]]
- [[Crafting System]]
