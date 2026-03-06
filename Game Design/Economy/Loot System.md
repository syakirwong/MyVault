---
tags:
  - gdd
  - economy
  - loot
---

# Loot System

## Loot Philosophy

Loot serves the build, not the other way around. Every drop should prompt the question: *"Does this change my build?"* — not *"Is this a bigger number?"*

The loot system reinforces three design pillars:

1. **Gems are the prize** — gems and crafting materials dominate loot tables; gear is secondary
2. **Targeted farming** — players know what drops where and farm with intent, not hope
3. **Crafting as pity** — if RNG doesn't deliver, the crafting path always exists as a deterministic alternative

Legendary gems never drop. They are crafted from components earned through the hardest content. The journey to a Legendary is always intentional.

---

## Loot Tables by Content Type

### Farming Dungeons (5-10 min, 1-3 players)

| Category | Drops per Run | Distribution |
|----------|--------------|--------------|
| Gems | 3-5 | Common 65%, Uncommon 25%, Rare 10% |
| Crafting Materials | 4-8 | Raw and Refined tier, domain-specific |
| Currency | 80-120 gold, 5-8 shards | Flat rate |
| Catalysts | Rare chance (~5%) | Minor tier only |

**Design intent:** High volume, low ceiling. Feed the crafting loop and give players targeted gem farming. Speed and efficiency are the skill expression here.

### Expedition Dungeons (20-30 min, 3-5 players)

| Category | Drops per Run | Distribution |
|----------|--------------|--------------|
| Gems | 4-6 | Uncommon 40%, Rare 40%, Epic 15%, Epic+ 5% |
| Crafting Materials | 6-10 | Refined and Pristine tier |
| Currency | 200-300 gold, 5-8 shards | Performance-modified (see below) |
| Catalysts | Moderate chance (~15%) | Minor and Standard tier |
| Gear | Rare chance (~10%) | Uncommon-Rare base stat pieces |

**Design intent:** The core loot experience. Balanced gem rarity with a real shot at Epic. Materials feed mid-to-high tier crafting. Group coordination improves drop quality.

### Mythic Dungeons (30-45 min, 5 players)

| Category | Drops per Run | Distribution |
|----------|--------------|--------------|
| Gems | 1-3 | Rare 30%, Epic 50%, Epic+ 20% |
| Legendary Components | 0-1 | ~15% chance per clear (see Legendary Pipeline) |
| Crafting Materials | 8-12 | Pristine and Mythic tier |
| Currency | 300-450 gold, 8-12 shards, 1-2 Resonance Tokens | Performance-modified |
| Catalysts | High chance (~30%) | Standard and Major tier |
| Gear | Moderate chance (~15%) | Rare-Masterwork base stat pieces |

**Design intent:** Fewer gems, higher floor. The primary source of Mythic-tier materials and Legendary components. Every Mythic clear advances the Legendary crafting path.

### Solo Trials (10-15 min, 1 player)

| Category | Drops per Run | Distribution |
|----------|--------------|--------------|
| Gems | 1-2 | Rare modifier gems (trial-specific) |
| Currency | Mastery XP bonus (1.5x) | Primary reward is mastery acceleration |
| Cosmetics | Rank-based | Trial-specific auras and titles |

**Design intent:** Skill expression, not loot farming. Rewards are mastery XP and prestige. The gems that drop are rare modifiers not available elsewhere.

### Open World

| Activity | Drops | Notes |
|----------|-------|-------|
| Resource Nodes | Raw materials, domain materials | Abundant, feeds crafting baseline |
| World Bosses | 5-10 shards, 1 Resonance Token, Rare gem chance | Guaranteed contribution reward |
| Daily Bounties | 50-100 gold, 2-3 shards, Common-Uncommon gems | Short session filler |
| Hidden Mini-Dungeons | Cosmetics, lore collectibles, occasional Uncommon gems | Exploration reward |

---

## Legendary Pipeline

Legendary gems are **strictly craft-exclusive**. They never drop from any content. Mythic dungeons and endgame activities provide the rare components required to craft them.

### Components Required

| Component | Source | Drop Rate | Needed |
|-----------|--------|-----------|--------|
| Mythic Materials | Mythic dungeons, High-risk dungeons | Common in Mythic loot | 10 |
| Resonance Tokens | Mythic bosses, High-risk completions, World bosses | 1-2 per Mythic clear | 5 |
| Legendary Catalyst | Mythic dungeon boss (rare drop) | ~15% per Mythic clear | 1 |
| Domain Essence | Essence Extraction (Salvaging Lv 40) from Epic+ gems | Deterministic (crafting) | 3 |

### Expected Path to Legendary

| Metric | Value |
|--------|-------|
| Mythic clears for materials | ~5-8 clears |
| Mythic clears for Legendary Catalyst (expected) | ~7 clears (at 15%) |
| Total Mythic clears (expected) | ~8-12 |
| Time investment (at 30-45 min each) | ~6-9 hours of Mythic content |
| Crafting level required | Gem Shaping 50 |

This is intentionally a multi-session journey. The Legendary Catalyst is the bottleneck — it drops only from Mythic bosses, cannot be crafted, and cannot be purchased. Everything else can be acquired through parallel paths.

---

## Bad Luck Protection

The crafting path is the primary pity mechanism — if drops don't cooperate, players can craft what they need from accumulated materials. However, a soft pity system exists for high-rarity drops:

### Escalating Drop Chance

| Item | Base Rate | Escalation | Cap |
|------|-----------|------------|-----|
| Epic gem | 4% per gem slot | +1% per Expedition clear with no Epic | 12% (after ~8 dry runs) |
| Legendary Catalyst | 15% per Mythic clear | +3% per Mythic clear with no Catalyst | 30% (after ~5 dry runs) |

**Rules:**
- Escalation resets on drop
- Escalation is per-player, tracked server-side
- Escalation does NOT apply to Farming Dungeons (those are volume-based, not rarity-based)
- Players are never told the exact escalation number — they just feel "luckier" over time

---

## Drop Composition Ratio

Per dungeon run, the loot table weights roughly:

| Category | Weight | Rationale |
|----------|--------|-----------|
| Crafting Materials | 50-60% | Feed the crafting economy, always useful |
| Gems | 30-40% | The highlight drops, build-defining moments |
| Currency (gold, shards, tokens) | 10% | Steady income, never the main event |

Catalysts are a bonus roll on top of the base table — they don't compete with gem or material slots.

---

## Gem Drop Identity

Gems drop **pre-determined** by the dungeon's drop table:

- **Domain** is fixed by dungeon ecology (Storm Cavern drops Storm gems, not Void gems)
- **Rarity** is rolled at drop time from the content's rarity distribution
- **Mutation state** is always Stable (100/100) on drop — mutations happen through combat use
- **Mastery** starts at 1

### Optional: Raw Gem Refinement

Players may encounter **Raw Gem** drops (~10% of gem drops). These are domain-unlocked:

| Property | Detail |
|----------|--------|
| Source | Any dungeon, replaces a normal gem drop |
| Cost to Refine | 50-200 gold at NPC (scales with rarity) |
| Player Choice | Select domain at refinement |
| Rarity | Locked at drop time (cannot be changed) |
| Frequency | ~10% of gem drops |

Raw Gems give players agency over domain selection without undermining targeted farming. A player who needs a specific domain can farm for it OR hope for a Raw Gem and choose.

---

## Consumable Drop Rules

Catalysts can drop from dungeon content but are deliberately rare:

| Catalyst Tier | Drop Source | Drop Rate | Design Intent |
|---------------|-----------|-----------|---------------|
| Minor | Farming Dungeons, Expeditions | ~5-15% per run | Taste of the system, encourages crafting investment |
| Standard | Expeditions, Mythic | ~10-20% per run | Supplements crafting, never replaces it |
| Major | Mythic only | ~5-10% per run | Rare bonus, crafting remains primary source |

**Rule:** Dropped catalysts are always lower quality than crafted equivalents. A dropped Standard Stabilizer reduces stability loss by 40% vs. 50% for crafted. This preserves crafting demand while making drops feel useful.

---

## Group Loot Distribution

### Contribution-Based Personal Loot

All group content uses **personal loot** modified by contribution metrics. Each player receives their own independent loot roll — no competition, no drama.

### How Contribution Works

Each player's contribution score is calculated from combat performance:

| Metric | Weight | What It Measures |
|--------|--------|-----------------|
| Damage dealt | 25% | Offensive contribution |
| Damage mitigated/healed | 25% | Defensive contribution |
| Resonance triggers | 20% | Build synergy and mastery |
| Momentum average | 15% | Tactical consistency |
| Mechanics handled | 15% | Boss-specific objectives (interrupts, positioning) |

### Contribution Tiers

| Tier | Threshold | Loot Modifier |
|------|-----------|--------------|
| S (Exceptional) | Top contributor | +1 bonus gem drop, +15% rarity upgrade chance |
| A (Strong) | Above median | +10% rarity upgrade chance |
| B (Solid) | Median performance | Base loot (no modifier) |
| C (Minimal) | Below median but active | -1 gem drop (minimum 1) |
| AFK/Leech | Detected inactivity | No loot |

**Safeguards:**
- Contribution is normalized by role — a defensive build dealing less damage isn't penalized if it mitigated proportionally
- Minimum loot floor: every active participant gets at least 1 gem and base materials
- AFK detection triggers after 2 consecutive rounds of no input
- Contribution tiers are shown post-dungeon but NOT during — no mid-fight anxiety

---

## Solo vs. Group Efficiency

| Content | Solo Rate (loot/hr) | Group Rate (loot/hr) | Ratio |
|---------|---------------------|---------------------|-------|
| Farming Dungeons | Full efficiency | N/A (1-3 players, scales linearly) | 1:1 |
| Expedition Dungeons | Not available solo | ~15-20% more efficient than Farming | Group bonus |
| Mythic Dungeons | Not available solo | Highest loot/hr for rarest materials | Group exclusive |
| Solo Trials | Full efficiency | N/A (solo only) | Solo exclusive |

**Design intent:** Solo players progress through Farming Dungeons and Solo Trials at full efficiency. Group content offers access to higher-tier loot (not faster acquisition of the same loot). Solo players are never punished — they have a complete progression path through crafting that compensates for inaccessible group-only drops.

---

## Skill-Based Bonus Rewards

Combat performance modifies currency rewards (from [[Economy Model]]):

| Performance | Gold Modifier |
|-------------|--------------|
| Momentum averaged 7+ across all rounds | +15% gold |
| Resonance triggered during boss fight | +10% gold per resonance |
| Zero gem stability loss (clean run) | +20% gold |
| Rounds survived beyond expected | +5% per extra round |

These modifiers stack with contribution tier bonuses for group content.

---

## Loot Presentation

How loot is communicated matters as much as what drops.

### Drop Notification Tiers

| Rarity | Notification | Visibility |
|--------|-------------|-----------|
| Common | Quiet — added to inventory log | Player only |
| Uncommon | Subtle highlight in loot summary | Player only |
| Rare | Prominent callout with domain icon | Player only |
| Epic | Animated callout with sound cue | Party-wide notification |
| Legendary Component | Full-screen moment with lore text | Party-wide + server announcement |

### Post-Dungeon Loot Summary

```
LOOT SUMMARY — Stormspire (Expedition, Challenge Mode)
────────────────────────────────────────────────────
  GEMS:
    [Epic] Storm Strike gem (Tempest domain)
    [Rare] Chain Lightning gem (Tempest domain)
    [Uncommon] Static Shield gem (Tempest domain)
    [Raw Gem] Uncommon — unrefined (choose domain at NPC)

  MATERIALS:
    Pristine Storm Crystal x3
    Refined Tempest Shard x5
    Mutation Residue x2

  CATALYSTS:
    [Standard] Mutation Catalyst (Tempest bias)

  CURRENCY:
    Gold: 287 (+15% momentum bonus, +20% clean run)
    Shards: 7
    Resonance Tokens: 0

  CONTRIBUTION: A (Strong) — +10% rarity upgrade applied
────────────────────────────────────────────────────
```

---

## Economic Integration

The loot system connects to other economic systems:

| Loot Output | Feeds Into | System |
|-------------|-----------|--------|
| Gem drops | Build experimentation, NPC marketplace listings | [[Gem System]], [[Trade System]] |
| Crafting materials | Gem Shaping, Catalyst Brewing, Resonance Mapping | [[Crafting System]] |
| Unwanted gems | Salvaging for shards and materials | [[Crafting System]] |
| Currency | NPC purchases, crafting fees, insurance, storage | [[Economy Model]] |
| Legendary Components | Legendary gem crafting pipeline | [[Crafting System]] |
| Raw Gems | Player-chosen domain refinement at NPC | NPC service |

Every piece of loot has a purpose. Nothing is vendor trash by design — even Common gems salvage into shards that fuel crafting.

---

## Related Documents

- [[Economy Model]]
- [[Crafting System]]
- [[Trade System]]
- [[Gem System]]
- [[PvE Structure]]
- [[Progression Systems]]
- [[Gameplay Loops]]
