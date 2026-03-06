---
tags:
  - gdd
  - core
  - factions
  - social
---

# Factions System

## Design Philosophy

Factions are **lateral power**, not vertical. Faction reputation unlocks new options — recipes, cosmetics, trade advantages, exclusive resonances — but never raw stat superiority. A player with zero faction reputation can still clear all content and compete in PvP. Faction investment makes you more *versatile*, not more *powerful*.

Factions also serve as **social anchors**. They give players communities within the community, rivalries that aren't toxic, and long-term identity beyond their build.

---

## Faction Overview

There are **6 factions** organized into two tiers:

### Major Factions (3)

Tied to the three alignments. These are the world's power structures.

| Faction | Alignment | Headquarters | Leader |
|---|---|---|---|
| **The Luminari** | Radiant | Radiant Sanctum, Celestial Peaks | High Luminar Sera |
| **The Hollow Court** | Corrupted | Corruption Font, Obsidian Wastes | The Hollow King |
| **The Driftborn** | Unbound | Druid Circle, Verdant Depths (roaming) | The Driftmind |

### Trade Factions (3)

Tied to the economy and crafting. These are the world's infrastructure.

| Faction | Focus | Headquarters | Leader |
|---|---|---|---|
| **Port Haven Trade Guild** | Commerce, auction house, trade routes | Port Haven, Calm Shores | Harbormaster Finn |
| **Stormreach Arena League** | PvP competition, ranked play, combat training | Thunder Arena, Stormreach | Arena Warden Zara |
| **The Gemcutter's Circle** | Crafting, gem research, material refinement | Lightning Forge, Stormreach | Lumen |

---

## Reputation System

### Reputation Tiers

All factions share the same tier structure. Points are earned through faction-specific activities.

| Tier | Points Required | Cumulative | Estimated Time |
|---|---|---|---|
| **Stranger** | 0 | 0 | Start |
| **Known** | 2,000 | 2,000 | 3-5 hours |
| **Trusted** | 5,000 | 7,000 | 10-15 hours |
| **Honored** | 10,000 | 17,000 | 25-35 hours |
| **Exalted** | 15,000 | 32,000 | 50-70 hours |

### Reputation Sources

| Activity | Rep Gained | Factions |
|---|---|---|
| Faction daily quest | 200-400 | Specific faction |
| Faction story quest | 500-1,500 | Specific faction |
| Dungeon completion (faction-aligned zone) | 100-300 | Regional faction |
| PvP match (ranked) | 150-300 | Arena League |
| Crafting (high-tier recipe) | 100-200 | Gemcutter's Circle |
| Auction house trade (completed) | 50-100 | Trade Guild |
| Trade route completion | 200-400 | Trade Guild |
| World boss kill | 100-250 | Regional major faction |
| Alignment quest | 200-500 | Aligned major faction |

### Reputation Decay

- **No decay.** Once earned, reputation is permanent
- **Exception:** Switching alignment resets your major faction reputation by 50% for the faction you're leaving. Trade faction reputation is never affected by alignment switches

---

## Major Faction Details

### The Luminari (Radiant)

> *"Light endures. Light protects. Light remembers."*

**Identity:** Order, protection, community. The Luminari believe in structure — guilds, hierarchies, collective defense. They view the void as a corruption to be cleansed and the Driftborn as dangerously naive.

**Reputation Rewards:**

| Tier | Reward | Category |
|---|---|---|
| Known | **Radiant Sigil** — faction badge on nameplate | Cosmetic |
| Known | **Sanctuary Recipes** — defensive gear with +Resilience focus | Crafting |
| Trusted | **Blessing of Light** — +10% healing received from all sources while in Radiant-aligned zones | Zone Bonus |
| Trusted | **Luminari Shield Gem** (exclusive) — Aegis domain variant: Counter-Energy converts to party-wide shields instead of personal damage | Exclusive Gem |
| Honored | **Radiant Resonance: Consecration** — Aegis + Resonance combination creates a healing echo field instead of damage echo. Only available to Luminari Honored+ | Exclusive Resonance |
| Honored | **Lightforged Gear** — Luminari visual set (gold trim, light particles). Masterwork tier stats | Cosmetic + Gear |
| Exalted | **Paragon's Aura** — visible radiant aura on character. Other Luminari players can see you on the zone map | Cosmetic + Social |
| Exalted | **Dawn's Edge** — legendary weapon recipe exclusive to Luminari Exalted. Grants +5% damage to Corrupted-aligned enemies | Exclusive Recipe |
| Exalted | **Luminari War Council** — access to faction-wide strategy chat and weekly objectives | Social |

**Specialty: Sustained Group Play**
The Luminari excel at long fights and group content. Their bonuses favor healing, protection, and durability. In extended dungeon runs and mythic content, Luminari players are the backbone.

---

### The Hollow Court (Corrupted)

> *"Power is not given. It is taken. It is eaten. It is worn."*

**Identity:** Ambition, risk, individual supremacy. The Hollow Court believes strength justifies itself. They don't see corruption as evil — they see it as evolution. The weak fear it; the worthy wield it.

**Reputation Rewards:**

| Tier | Reward | Category |
|---|---|---|
| Known | **Void Mark** — faction badge on nameplate (pulses when near other Corrupted players) | Cosmetic |
| Known | **Corruption Recipes** — offensive gear with +Force and +Acuity focus | Crafting |
| Trusted | **Void Hunger** — +10% damage dealt to enemies below 30% Integrity while in Corrupted-aligned zones | Zone Bonus |
| Trusted | **Hollow Siphon Gem** (exclusive) — Void domain variant: Consume action now steals stability from target gem instead of buffs | Exclusive Gem |
| Honored | **Corrupted Resonance: Unmaking** — Entropy + Void combination has a 15% chance to permanently reduce target gem mastery by 1 level (PvE bosses only — reduces boss gem tier). Only available to Hollow Court Honored+ | Exclusive Resonance |
| Honored | **Voidwoven Gear** — Hollow Court visual set (purple/crimson, shadow particles). Masterwork tier stats | Cosmetic + Gear |
| Exalted | **Corruption Haze** — visible void distortion around character. Ambient NPCs react with fear | Cosmetic + Social |
| Exalted | **Abyssal Edge** — legendary weapon recipe exclusive to Hollow Court Exalted. Self-damage from Corrupted alignment is reduced by 25% | Exclusive Recipe |
| Exalted | **Court of Whispers** — access to faction-wide intel chat and bounty system (mark enemies for other Court members) | Social |

**Specialty: Burst Damage and Risk-Reward**
The Hollow Court excels at ending fights fast. Their bonuses amplify damage and add unique resource-stealing mechanics. In PvP and speed-clearing dungeons, Corrupted players set the pace.

---

### The Driftborn (Unbound)

> *"Neither the light nor the void sees clearly. Only distance reveals the full shape."*

**Identity:** Flexibility, diplomacy, hybrid mastery. The Driftborn refuse to choose sides. They believe balance requires understanding all forces. They serve as mediators, traders, and wild cards.

**Reputation Rewards:**

| Tier | Reward | Category |
|---|---|---|
| Known | **Balance Sigil** — faction badge on nameplate (shifts color based on recent gem usage) | Cosmetic |
| Known | **Adaptive Recipes** — hybrid gear that provides 3 stats instead of 2 (lower per-stat values) | Crafting |
| Trusted | **Flow State** — cooldown reduction bonus increases from 20% to 25% while in neutral zones | Zone Bonus |
| Trusted | **Driftweave Gem** (exclusive) — Flux domain variant: Metamorphosis now lets you choose the replacement action set instead of random | Exclusive Gem |
| Honored | **Driftborn Resonance: Convergence** — any two domains with 50%+ stability create a temporary "third resonance" that mirrors the most recently triggered resonance on the field (yours or opponent's). Only available to Driftborn Honored+ | Exclusive Resonance |
| Honored | **Driftweave Gear** — Driftborn visual set (silver/blue, shimmering particles). Masterwork tier stats | Cosmetic + Gear |
| Exalted | **Neutral Aura** — visible shimmer. Both Luminari and Hollow Court faction leaders acknowledge you with unique dialogue | Cosmetic + Social |
| Exalted | **Equilibrium Blade** — legendary weapon recipe exclusive to Driftborn Exalted. Grants +3% all stats (applies to every stat equally) | Exclusive Recipe |
| Exalted | **The Third Path Council** — access to cross-faction diplomacy chat. Can participate in both Luminari and Hollow Court weekly objectives (reduced rep gain) | Social |

**Specialty: Versatility and Information**
The Driftborn excel at adaptation. Their bonuses improve cooldowns, grant action choice, and mirror opponent strategies. In unpredictable content (Mythic modifiers, PvP against unknown builds), Driftborn players have the widest toolkit.

---

## Trade Faction Details

### Port Haven Trade Guild

> *"Gold flows like water. We control the current."*

**Focus:** Economy, trade routes, market intelligence.

**Reputation Rewards:**

| Tier | Reward | Category |
|---|---|---|
| Known | Auction house listing fee reduced by 5% | Economy |
| Known | Access to **Trade Route Contracts** (escort cargo for materials + gold) | Content |
| Trusted | AH fee reduced by 10%. Access to **Price Historian Dael's** advanced market data | Economy + Info |
| Trusted | **Merchant's Signet** — gear accessory: +5 Acuity, +5 Instinct, +10% gold from all sources | Gear |
| Honored | AH fee reduced by 15%. Can post **bulk listings** (sell stacks of materials as single lots) | Economy |
| Honored | **Trade Magnate** title. Guild storefront visibility boost (+25% more views on listed items) | Social + Economy |
| Exalted | AH fee reduced by 20%. Access to **Black Market** — rare materials unavailable elsewhere, refreshes weekly | Economy + Exclusive |
| Exalted | **Golden Seal of Commerce** — legendary accessory recipe: +10 to two stats of choice, +15% gold from trade routes | Exclusive Recipe |

---

### Stormreach Arena League

> *"Rank is truth. Everything else is opinion."*

**Focus:** PvP competition, combat mastery, ranked play.

**Reputation Rewards:**

| Tier | Reward | Category |
|---|---|---|
| Known | PvP honor gains increased by 10% | PvP |
| Known | Access to **Arena Training Dummies** — simulate PvP against saved build snapshots | Content |
| Trusted | PvP honor gains +15%. **Arena Veteran** title. Access to **weekly tournament queue** | PvP + Content |
| Trusted | **Gladiator's Band** — gear accessory: +5 Precision, +5 Force, +5% momentum gain in PvP | Gear |
| Honored | **Arena Champion** title options (per season). Access to **Spectator Mode** with enhanced combat log | PvP + Social |
| Honored | **Arena Resonance: Momentum Surge** — at Momentum 8+, your next action deals +10% damage. PvP only | Exclusive Bonus |
| Exalted | **Grand Champion** title. **Arena Legend Aura** — exclusive PvP visual effect | Cosmetic |
| Exalted | **Stormforged Gauntlets** — legendary weapon recipe: +8 Force, +8 Precision, speed priority +1 in PvP | Exclusive Recipe |

---

### The Gemcutter's Circle

> *"Every gem has a voice. We teach you to listen."*

**Focus:** Crafting mastery, gem research, material science.

**Reputation Rewards:**

| Tier | Reward | Category |
|---|---|---|
| Known | Crafting speed +10% (reduced crafting time) | Crafting |
| Known | Access to **Gem Appraisal** — reveals hidden mutation potential on unmastered gems | Content |
| Trusted | Crafting speed +20%. **Journeyman** title. Access to **advanced recipes** (Epic-tier gems) | Crafting |
| Trusted | **Gemcutter's Loupe** — gear accessory: +5 Attunement, +5 Acuity, crafted gems start at Mastery 1 instead of 0 | Gear |
| Honored | **Master Crafter** title. Access to **Experimental Crafting** — chance to create gems with pre-applied mutations | Crafting + Unique |
| Honored | **Circle Resonance: Refined Stability** — all equipped gems start combat at 105 stability instead of 100 | Exclusive Bonus |
| Exalted | **Grandmaster** title. **Crafter's Signature** — your name permanently attached to any Legendary item you craft | Social + Prestige |
| Exalted | **Philosopher's Lens** — legendary accessory recipe: +10 Attunement, +5 all other stats, resonance discovery chance +15% | Exclusive Recipe |

---

## Cross-Faction Dynamics

### Major Faction Tensions

| Pair | Relationship | Mechanical Effect |
|---|---|---|
| Luminari vs Hollow Court | Hostile | +10% damage against each other in open-world PvP. Cannot trade directly |
| Luminari vs Driftborn | Wary respect | No mechanical bonus/penalty. Full trading |
| Hollow Court vs Driftborn | Uneasy tolerance | No mechanical bonus/penalty. Full trading |
| Same faction | Allied | +5% XP when grouping with same-faction players. Faction party buffs available |

### Trade Faction Independence

- Trade factions have **no alignment requirement**. A Corrupted player can be Exalted with the Gemcutter's Circle
- Trade factions have **no conflicts with each other**. You can be Exalted with all three simultaneously
- Trade faction reputation is **never lost** due to alignment switches

### Multi-Faction Strategy

Players can invest in multiple factions simultaneously. The question isn't *which* faction — it's *how deep* and *in what order*.

| Player Type | Recommended Faction Priority | Reasoning |
|---|---|---|
| PvP-focused | Arena League → Major (any) → Circle | PvP bonuses first, then alignment power, then crafting |
| Crafter/Trader | Circle → Trade Guild → Major (any) | Crafting recipes, then economy bonuses, then content access |
| PvE progression | Major (aligned) → Circle → Trade Guild | Exclusive gems/resonances for dungeon content, then crafting |
| Social/Explorer | Major (any) → all three trade factions | Breadth over depth, access to all communities |

---

## Faction Daily Quests

Each faction offers 1-2 daily quests. These are the primary repeatable source of faction reputation.

### Major Faction Dailies

| Faction | Quest Type | Example | Rep Reward |
|---|---|---|---|
| Luminari | Protection | Defend a settlement from corruption wave (PvE) | 300 |
| Luminari | Healing | Restore a corrupted zone node (gathering + puzzle) | 200 |
| Hollow Court | Elimination | Kill a Radiant-aligned world elite (PvE) | 300 |
| Hollow Court | Corruption | Channel void energy at 3 ley line nodes (exploration) | 200 |
| Driftborn | Mediation | Complete a cross-faction NPC dispute (dialogue choices) | 300 |
| Driftborn | Balance | Use all 3 alignment types of gems in a single dungeon run | 200 |

### Trade Faction Dailies

| Faction | Quest Type | Example | Rep Reward |
|---|---|---|---|
| Trade Guild | Delivery | Escort cargo from Port Haven to Stormreach | 250 |
| Trade Guild | Market | Buy and resell 3 items at a profit within 24 hours | 200 |
| Arena League | Match | Win 2 ranked PvP matches | 300 |
| Arena League | Training | Complete a solo trial with a score above threshold | 200 |
| Circle | Crafting | Craft 3 items of Rare+ quality | 250 |
| Circle | Research | Appraise 5 gems (use appraisal tool on unidentified drops) | 200 |

---

## Faction Events (Weekly/Seasonal)

### Alignment Wars (Weekly)

The three major factions compete for territory control across contested zones.

- **Duration:** 7-day cycles
- **Mechanic:** Players complete PvE and PvP objectives in contested zones to earn faction war points
- **Victory:** Winning faction gains a zone-wide 5% XP bonus and a cosmetic banner in all hubs for the following week
- **Participation reward:** All participants earn bonus faction rep regardless of outcome (500-1,000)

### Trade Festival (Monthly)

All three trade factions celebrate commerce.

- **Duration:** 3-day event
- **Mechanic:** Boosted crafting yields, reduced AH fees for everyone, special limited-time recipes
- **Exclusive:** Festival cosmetics (themed gear skins, title: "Festival Patron")
- **Rep bonus:** All trade faction rep gains doubled during the festival

### Season Championships (Quarterly)

Arena League hosts a major tournament.

- **Duration:** 1-week qualifier + weekend finals
- **Mechanic:** Top 64 players compete in bracketed tournament with spectator mode
- **Exclusive:** Seasonal champion title, exclusive arena aura, unique weapon skin
- **Rep bonus:** Significant Arena League reputation for all ranked participants

---

## Faction & Guild Interaction

Guilds can declare a **faction affiliation** (one major faction). This unlocks:

| Feature | Requirement | Effect |
|---|---|---|
| **Faction Guild Banner** | Any affiliation | Guild banner displays faction colors and sigil |
| **Faction Rally** | 10+ guild members at Trusted+ | +3% all stats for guild members in faction-aligned zones |
| **Faction Quartermaster** | 5+ guild members at Honored+ | Guild hall gains a faction vendor with discounted faction recipes |
| **Faction Champion Slot** | Guild leader at Exalted | Guild gains 1 exclusive "Faction Champion" title for their top PvP player |

Guilds that switch faction affiliation lose their faction-specific features and must re-earn them. This creates meaningful commitment.

---

## Balance Guardrails

| Rule | Rationale |
|---|---|
| No faction provides raw stat bonuses above +10 per item | Prevents faction stacking from overshadowing gem builds |
| Exclusive gems are variants, not upgrades — they trade one behavior for another | No faction gem is strictly better than the base domain version |
| Exclusive resonances require Honored+ (25-35 hours of faction play) | Deep investment required, can't be trivially accessed |
| Zone bonuses only apply in faction-aligned zones, not globally | Geographic advantage, not universal power |
| PvP bonuses are capped at +10% in any single dimension | Prevents faction choice from deciding PvP outcomes |
| Trade faction bonuses are economic/convenience, never combat power | Economy players are never forced into PvP; PvP players never forced into trading |

---

**Related:** [[Alignment System]] | [[NPC System]] | [[Progression Systems]] | [[Experience & Levelling]] | [[Stats System]] | [[Social Hubs]] | [[PvP Structure]]