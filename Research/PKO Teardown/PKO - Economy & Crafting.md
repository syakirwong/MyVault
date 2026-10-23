---
tags:
  - research
  - pko
  - economy
source: https://piratekings.online/guide/
captured: 2026-10-23
---

# PKO - Economy # PKO Teardown: Items, Economy and Crafting Crafting

Source: 21 scraped player guides from a PKO (Tales of Pirates) private server. These numbers describe **that server's configuration** and may differ from retail TOP or other servers. Lines marked **[Derived]** are my own calculations. Lines marked **[Uncertain]** are places where the guide is ambiguous or doesn't say.

Currency: **Gold (G)** is the soft currency. **Rum** is the Item Mall premium currency. Several core systems are deliberately gated behind Rum items.

---

## 1. Items and Gold (foundation)

### How it works
- **Item classes:** common drops (mostly vendor trash or quest fodder), consumables (HP/SP restores and timed buffs), equipment (restricted by race, class and level; can be forged), and quest items (bound to a quest).
- **Pickup:** click the item or press Ctrl+A to grab everything nearby. Most items can be dropped, but some are flagged undroppable.
- **Player trade:** right-click a player to trade (needs Lv6+). It's a two-step handshake: Confirm shows your side, then both players press Trade. It only completes if both inventories have room, and gold can go in the window. Untradeable items usually can't be dropped either.
- **Gold uses:** repair, teleport, forging, NPC purchases, and above all player-to-player buying.
- **Gold sources:** early on, mainly selling mob drops to NPCs. Later, the guides say most gold comes from trading on the player market. Items sold to an NPC **cannot be bought back**.

### Design takeaways
- The only real gold **faucet** is selling drops to NPCs. Everything else just moves gold between players, so the money supply depends on how often players kill mobs and what vendors pay.
- The Lv6 trade gate is a cheap defence against bots and throwaway mule accounts. Add it on day one.
- The guides warn players to check trades by hand, which suggests the trade UI has no verification support. A "you are receiving X" diff screen plus a short cooldown before confirming is cheap protection against scams.

---

## 2. Gems: types, stats and sources (core reference)

### How it works
Gems are the main way characters scale. Each gem has a **family** (the stat it gives), a **level** (Lv1-5 on this server, or Lv1-3 for Black Dragon gems), allowed **slots** (which gear types accept it), and a **glow colour** (shown on weapons only).

**Attribute gems** (common, "budget" tier)

| Gem | Slots | Bonus per level* | Glow |
|---|---|---|---|
| Fiery | 1H swords | +4 ATK | Red |
| Furious | Greatsword, bow, staff, dagger | +6 ATK | Red |
| Explosive | Guns | +10 ATK | Red |
| Spirit | All weapons, shoes | +1 SPR | Yellow |
| Shadow | All weapons, shoes | +2 Dodge | Blue |
| Lustrious | All weapons, gloves | +5 Hit | Red |
| Shining | Armor, shield | +100 HP | none |
| Glowing | Armor, shield | +5 DEF | none |

*The guide gives each gem's bonus as one value, and its forging example shows +5 DEF from a Lv1 Glowing gem. I'm assuming the bonus scales linearly with level, but the guides never state the Lv2-5 values. **[Uncertain]**

Note: the ATK values are balanced per weapon class (guns get +10, 1H swords +4). The gem roughly cancels out differences in weapon speed or base damage.

**Unique gems** (the only reliable source of primary stats), with quality tiers:

| Family | Slots | Broken | Cracked | Chipped | Unique |
|---|---|---|---|---|---|
| Rage | All weapons | +2 STR | +3 | +4 | +5 |
| Wind | Shoes | +2 AGI | +3 | +4 | +5 |
| Colossus | Armor, shield | +2 CON | +3 | +4 | +5 |
| Soul | All weapons | +2 SPR | +3 | +4 | +5 |
| Striking | Gloves | +2 ACC | +3 | +4 | +5 |

- **Exclusivity rule:** an item can't hold two quality variants of the same family (for example Broken + Cracked Rage).
- **Sources:** Broken comes from Novice Chaos Chests (also Wowo Treasures, Locke's maps and Sheepskin scrolls), Cracked from Standard chests, Chipped from Expert chests, and Unique from Maze bosses, Legendary Chaos chests, Sheepskin scrolls and Black Dragon mobs. Chest tier maps directly to gem quality.

**Black Dragon gems** (top tier, **capped at Lv3**)

| Gem | Slots | Bonus | Glow |
|---|---|---|---|
| Eye of Black Dragon | All weapons | +50 ATK | Red |
| Heart of Black Dragon | Gloves, shoes | +500 HP | none |
| Soul of Black Dragon | Armor, shield | +3 Physical Resist | none |

These drop only from Black Dragon and its minions. The lower level cap is how the game balances their large per-level bonus.

**Refining gems ("Ref")** give no stats. They are consumed as a catalyst when forging, and their level must match the gem being forged. Sources: life quests, story quests, salvage, Forsaken City and Dark Swamp maze bosses and mobs, high-level sea mobs, and Chaos Argent.

**Slot coverage (who can use what):**

| Slot | Gems allowed |
|---|---|
| Weapon | Fiery/Furious/Explosive, Spirit, Shadow, Lustrious, Rage, Soul, Eye of BD |
| Armor / shield | Shining, Glowing, Colossus, Soul of BD |
| Gloves | Lustrious, Striking, Heart of BD |
| Shoes | Spirit, Shadow, Wind, Heart of BD |
| Hat / ring / necklace / coral | Can be socketed, but **no gem fits** |

### Design takeaways
- **Each gem fits only certain slots**, so every slot has its own small menu (shoes are the AGI slot, gloves the ACC slot, and so on). That gives build choices without letting players stack one stat everywhere. If your game is built around gems, this is the main lever to copy.
- **Hats, rings and necklaces take sockets but no gem fits them.** This reads like unfinished content, and it confuses players. Either give every socketable slot at least one gem or make those slots unsocketable.
- **Three power tiers with separate rules:** Attribute (cheap, Lv5), Unique (stats, Lv5, quality ladder) and Black Dragon (huge, Lv3 cap, one source). Capping the strongest tier at a lower level keeps its ceiling in check.
- **Gem colours make weapons glow,** which works as both a power display and a status symbol. It costs almost nothing and gives players a reason to chase a "glow build".
- **Same-icon scam risk:** Lustrious and Rage share an icon, and the guide flags this as a scam vector. Give every gem family a distinct icon.

---

## 3. Gem Combining (level up a gem: 2 to 1)

### How it works
At Grocery - Amos: **2 gems of the same family and level, a Gem Composition Scroll (5,000G) and 50,000G** give one gem a level higher. Refining gems follow the same rules with their own success table. If a combine fails, **both gems, the scroll and the gold are lost.**

| Target level | Gem success | Gem with Red Fruit (+20) | Ref success | Ref with Red Fruit |
|---|---|---|---|---|
| Lv2 | 90% | 100%† | 100% | 100% |
| Lv3 | 80% | 100% | 100% | 100% |
| Lv4 | 70% | 90% | 95% | 100% |
| Lv5 | 60% | 80% | 90% | 100% |
| Lv6-9 (table lists them; server cap is Lv5) | 50/40/30/20% | 70/60/50/40% | 85/80/75/70% | 100% |

†I'm assuming the +20 is added in percentage points and capped at 100%. **[Uncertain]**

- **Red Composition Fruit:** adds +20 to gem combines (makes refining gem combines 100%) for **1 minute**, covering every combine done in that window. It comes only from story quests, so it's scarce. The best play is to batch all your Lv3+ combines inside one Fruit window.
- **Best-case cost table (as the guide gives it):** Lv N needs 2^(N-1) Lv1 gems. Lv5 needs 16 Lv1 gems plus 750k gold in fees (15 combines × 50k), not counting scrolls.

**[Derived] Expected Lv1-gem cost and gold (fees plus scrolls at 55k per try) once failures are included:**

| Target | Gems, no fruit | Gems, with fruit | Refs, no fruit | Refs, with fruit | Gold, gem, no fruit |
|---|---|---|---|---|---|
| Lv2 | 2.2 | 2.0 | 2.0 | 2.0 | 61k |
| Lv3 | 5.6 | 4.0 | 4.0 | 4.0 | 222k |
| Lv4 | 15.9 | 8.9 | 8.4 | 8.0 | 712k |
| Lv5 | **52.9** | **22.2** | 18.7 | 16.0 | **2.46M** |

The Fruit cuts the cost of a Lv5 gem by about 2.4×, because failures compound with each level.

### Design takeaways
- **A 2:1 merge with a failure chance turns cost exponential.** The best case for Lv5 is 16 gems, but the expected cost is 53. That is an enormous gem sink, and it's what keeps low-level gems worth something for years.
- **Timed buff windows ("use within 1 minute") encourage batch crafting sessions,** which concentrate market demand into bursts. They work well as scarce quest rewards.
- **Refining gems use gentler odds than stat gems.** The catalyst should be the reliable part of the chain and the stat gem the risky part, so the frustration lands on the item players care about.
- For your build system, consider adding pity or a partial refund (for example, a failure returns one gem), or the top tier ends up being something only botters reach.

---

## 4. Gem Upgrade (quality ladder: Broken → Cracked → Chipped → Unique)

### How it works
At Amos: **2 gems of the same quality and level, a Gem Upgrade Scroll (5,000G) and 50,000G** give the next quality tier at the same level, with a **100% success rate**.

**[Derived] Stat efficiency at a fixed level:**

| Quality | Stat | Broken gems consumed | Stat per Broken |
|---|---|---|---|
| Broken | +2 | 1 | 2.0 |
| Cracked | +3 | 2 | 1.5 |
| Chipped | +4 | 4 | 1.0 |
| Unique | +5 | 8 | 0.63 |

So one Unique costs 8 Broken gems of the same level, plus 7 upgrades (about 385k gold). Returns per tier are strongly diminishing, and that's intentional: Unique pays off because each item has only 2 sockets, not because it's efficient per gem.

### Design takeaways
- **This is a second axis of progression:** quality is separate from level. Combined, they form a 2D grid (quality × level). That gives far more steps than a single ladder, and every chest tier's drops stay useful.
- **Deterministic upgrades (100%) beside risky level-ups (60-90%)** is a good contrast. Players get a safe path to spend surplus on and a risky one for prestige.
- **Weak gems are efficient per gem, strong gems are efficient per socket.** Because sockets are scarce, this tradeoff is the most portable idea in the whole system.

---

## 5. Sockets

### How it works
- Up to **2 sockets** per item (3 on very rare drops, which can't be made by hand).
- You add sockets at Blacksmith - Smithy ("Fusion") using an **Equipment Stabilizer and Equipment Catalyst (25 Rum each)**. Gold cost is **50k for socket 1 and 100k for socket 2** (150k total), with a **100% success rate**. Whether one Stabilizer and Catalyst set is needed per socket isn't stated. **[Uncertain]**
- Drops, quest rewards, Newbie chests and Mystic/Beautiful chests can roll 1-2 sockets. Class chests, sealed or unsealed gear and Chaos Gearstones **never** roll sockets, so they have to be socketed by hand.
- Sockets carry through apparel fusion without a Fusion Catalyst.
- If you extract the gem from socket 1, the gem in socket 2 does not shift. The next forge fills socket 1 again.

### Design takeaways
- **Sockets are the gate you sell for real money (Rum).** Every top-end item eventually needs about 50 Rum to socket, because endgame gear is excluded from rolling sockets. This is clearly deliberate monetization.
- **A rare 3-socket drop** creates jackpot items worth far more than normal, which keeps players reading drop lists. Copy it, but keep it extremely rare.
- Two sockets is a tight budget, and that's what makes the slot whitelist and quality choices matter.

---

## 6. Equipment Forging (put gems into gear)

### How it works
At Smithy ("Forge"): **equipment + gem + matching-level refining gem + gold.** Levels can't be skipped. You forge Lv1 into an empty socket, then forge Lv2 of the same family over it, and so on. A higher-level gem than needed is treated as the next level, so the extra is wasted. Total gem levels show as "+X" on the item; two Lv5 sockets make **+10**.

- **Fee = target "+X" × 100,000G.** It depends on how many times the item has been forged in total, not on the gem's level. The second socket costs more because its "+" numbers start after the first socket's.
- **On failure:** you lose the gem, the refining gem and the gold. The **equipment is safe.**

| Forge level | Success | With Blue Forging Fruit (+40) |
|---|---|---|
| Lv1-3 | 100% | 100% |
| Lv4 | 45% | 85% |
| Lv5 | 35% | 75% |
| Lv6-9 (above server cap) | 25 / 15 / 5 / 2% | 65 / 55 / 45 / 42% |

- **Blue Forging Fruit:** +40 flat for 1 minute, from story quests only.
- **Glow tiers:** weapons glow from +1, with stronger effects at **+4, +7 and +10**. Each weapon type has its own glow progression, and the gem colour (red, yellow or blue) tints it.

**[Derived] Filling one socket from empty to Lv5, in expected Lv1-equivalents (combine and forge failures both included):**

| Scenario | Lv1 gems | Lv1 refs | Forge fees (socket 1, +1 to +5) |
|---|---|---|---|
| Best case (no failures) | 31 | 31 | 1.5M |
| No fruits | **~195** | ~79 | ~2.9M |
| Red and Blue fruits on every risky step | ~47 | ~38 | ~1.7M |
| No fruits, Insurance on Lv4/Lv5 forges | ~78 | ~79 | ~2.9M |

The second socket (+6 to +10) costs about **7.0M in forge fees alone** without fruits (600k+700k+800k+900k/0.45+1M/0.35). A full +10 weapon realistically costs several hundred Lv1 gems and 15M+ gold.

### Design takeaways
- **Failure destroys materials but never the gear.** This is the key trust decision. Players accept burning consumables, and destroying gear causes rage-quits. Copy it.
- **The fee grows with the item's total forges, not gem level,** so the second socket is a steeper sink and late-stage upgrades keep draining gold even though gems are cheap.
- **Glow milestones (+4/+7/+10)** sit just past the points where success rates drop. Visible status is tied to the riskiest steps, which drives demand for fruits, insurance and refining gems.
- **The 100% → 45% cliff at Lv4** is harsh. If you want smoother pacing, use a gradual curve such as 100/90/70/50/35.

---

## 7. Insurance Card (failure protection)

### How it works
- Sold in the Item Mall (Forging section) as **Lv4 and Lv5 cards**, used for Lv4 and Lv5 forges. Rum price not stated. **[Uncertain]**
- The card goes in the 4th forge slot. It **does not raise the success rate.** On a failure it **returns the gem**, but the refining gem (and presumably the gold) is still lost. The card is consumed whether the forge succeeds or fails.
- The guide recommends using it together with Blue Fruit.

### Design takeaways
- **Protection is sold as a premium item rather than odds,** so success rates stay "fair" while paying players lose less. This is the cleanest pay-to-reduce-variance model in the set.
- [Derived] With no fruits, insurance cuts the expected Lv1 gem cost for a Lv5 socket from about 195 to about 78. It's worth roughly 117 Lv1 gems, so you can price the card against gem market value.
- Because the refining gem is still lost, the free-to-play demand for refining gems survives even among paying players. Always leave one material at risk.

---

## 8. Gem Extraction ("Pliering")

### How it works
- At Furnace of Immortality (Spring Town, requires the Language Barrier quest). You need **Blacksmith Pliers (15 Rum)**, which pull socket 1 first then socket 2, or **Precise Pliers (29 Rum)**, which pull socket 2 first then socket 1.
- Gold: **10k per gem forged** into the item. **[Uncertain]** whether that's per gem level or per gem.
- **100% success.** Gems come out from the highest level down. **Refining gems are not refunded.** My reading is that each extraction step returns gems from the top level down, but the guide doesn't say exactly how many gems one plier use returns. **[Uncertain]**

### Design takeaways
- **Being able to remove gems keeps them liquid.** Players can move gems from old gear to new, so gem prices hold up as gear turns over.
- **The catalyst (refining gem) is the permanent sink.** Gems are kept, but the refs spent on forging are gone. That's a clean "you keep the asset, lose the labour" rule.
- The price gap for controlling which socket comes out first (15 vs 29 Rum) monetizes convenience without affecting power. It's low-friction monetization.

---

## 9. Apparel Fusion (transmog / cosmetics)

### How it works
- At Smithy ("Apparel Fusion", not "Fusion"). Inputs: **Fusion Scroll + apparel (left) + stat item (right) + gold**, plus a **Fusion Catalyst** only if the stat item is gemmed or upgraded.
- **Gold = 1,000 × the item's level requirement** (Lv65 item = 65k). **100% success.**
- The apparel takes the item's stats, level, class and gems, and **the source item is consumed.** The fused item's max durability also goes up.
- **Slot order matters.** Putting the items in the wrong slots overwrites a valuable apparel with an empty one and destroys the stats. The guide calls the losses "horrendous".
- Fusing new gear into a used apparel throws away the old stats.
- **Cross-race:** fusing into multi-race sets (Lance/Ami/Carsise with Phyliss) makes the item wearable by both races. Lucky Bull makes it wearable by all races (within class limits). Classless Chaos Gearstones can then be worn by all 4 races.
- It's the **only way to make Gearstones wearable.** Apparels come from the Item Mall, except the Aries set (Aries quest, "Captain" difficulty).
- Fusion Scroll sources: Item Mall, Beautiful chests, Forsaken City, Dark Swamp, and high-level sea mobs. Fusion Catalyst: Item Mall only.

### Design takeaways
- **Transmog is the gateway to the paid upgrade path:** apparel upgrade only works on fused items, so cosmetics bought with Rum become a required power step. That's effective and mildly predatory.
- **Overwriting through slot order is a UX trap.** Lock the "base" item clearly and show a preview of the result before confirming.
- **Cross-race apparel shells** let gear trade across races, which widens the market for any given item. It's a clever liquidity tool.

---

## 10. Apparel Upgrade (Strengthening)

### How it works
- Only for **fused** apparel. At Smithy (Magical Ocean): **Strengthening Scroll** (drops from sea and Forsaken City mobs, also sold in the Mall) + **Strengthening Crystal (7 Rum, Mall only)** + gold.
- Each success adds **+2% effectiveness**, up to **110%** (5 steps). The bonus applies to all stats, **including gem bonuses**, and is rounded down to whole numbers.
- **Success rate is a flat 50% per step. Fruits don't apply.** The failure penalty isn't stated beyond losing materials. **[Uncertain]**

| Step | Success | Gold |
|---|---|---|
| 100 → 102% | 50% | 1.21M |
| 102 → 104% | 50% | 1.44M |
| 104 → 106% | 50% | 1.69M |
| 106 → 108% | 50% | 1.96M |
| 108 → 110% | 50% | 2.25M |

[Derived] Each step's cost is (1.0 + 0.1k)² million. Reaching 110% has an expected cost of about **17.1M gold plus about 10 crystals (≈70 Rum)** and 10 scrolls.

### Design takeaways
- **A percentage multiplier on top of flat gem bonuses** makes gems more valuable the more you've invested. It's a late-game sink that scales with each player's wealth.
- **A flat 50% that fruits can't touch** sets this apart as a "pure gamble" sink. It works as a gold drain but is frustrating. Consider pity after N failures.
- Rounding down to whole numbers makes small percentage gains nearly invisible on low stats. Show the actual gain before the player commits.

---

## 11. Life Skills: Gathering (primary)

### How it works
- **Life skill points:** one every few story quests, plus 5 from the "Language Barrier" quest. Skills are bought from any Grocer for **1,000G** and levelled by spending points.
- Resource nodes are **monster-like entities** at levels 1-10. Each tool hit removes 1 HP and has a chance to drop loot until the node dies and respawns. Your skill level must be at least the node's level.

| Primary | Tool | Feeds secondary | Notes |
|---|---|---|---|
| Woodcutting | Axe (Blacksmith) | Manufacturing | Gold Axe (event reward) doubles yield. The Legendary Tree of Fortune can't be destroyed and allows multiple players at once. |
| Mining | Pickaxe | Crafting | The Meteorite node needs an Alloy Pickaxe (from the Language Barrier quest, or 29 Rum; can't be repaired). |
| Fishing | Ship part: Fishing Net | Cooking | No way to double the yield. |
| Salvage | Ship part: Mechanical Arm | Analyze (**not implemented**) | Some drops go to the ship's cargo hold. |

- While an axe or pickaxe is equipped, you can't attack or use most skills.

### Design takeaways
- **Treating nodes as "monsters" that lose 1 HP per hit** reuses the combat and drop systems. It's cheap to build, and yield becomes a drop table you can tune.
- **The two sea skills need ship parts,** which ties life skills to the ship system and creates demand for specific ship builds.
- **Shared nodes that never deplete** (Tree of Fortune, Meteorite) create social hotspots and are good event content.
- A secondary skill that was never built (Analyze) is dead weight. Don't ship parts of a skill tree that lead nowhere.

---

## 12. Production: Manufacturing, Crafting, Cooking (secondary)

### How it works
**Blueprints are consumable recipes with random stats.** Each one has: level, product, **always 3 ingredients**, a limited number of uses, a base success rate, and a Fairy Coin cost per use.

| Closed blueprint | Drop source | Opens to |
|---|---|---|
| Blurry | Low-level mobs, high rate | Lv1 100% |
| Encrypted | Low-level mobs, decent rate | Lv2 70% / Lv3 25% / Lv4 5% |
| Sealed | Higher-level mobs, low rate | Lv4 70% / Lv5 25% / Lv6 5% |
| Invocation | Very high-level mobs, very rare | Lv6 70% / Lv7 25% / Lv8 5% |

Type split when opened: **Manufacturing 40% / Crafting 20% / Cooking 40%.**

- **Skill levels:** the Lv1 book comes from a Grocer. Higher books are bought from the **Fairy Merchant** with Fairy Coins or Elven Signets, and levels can't be skipped. A secondary skill can't exceed its primary skill's level, and your skill level must be at least the blueprint's. **A higher skill level does not raise the success rate.** The guides say levels 1-7 in one place and list 8 tiers in another. **[Uncertain]**
- **Three-part setup:** player skill + a **fairy (pet)** with a Novice or Standard production skill (takes one of the pet's 3 skill slots; raises success) + a **tool**.
- **Tool:** 200 durability, repaired at the Furnace of Immortality with a Repair Tool of the same level. **Tools level up and higher tool levels raise success.** Each success gives EXP equal to the blueprint level; each failure gives 1 EXP. EXP needed per level is 100 / 400 / 900 / 1,600 (cumulative 3,000 to Lv5), which [Derived] follows 100 × (L−1)².
- **On failure you lose the materials only.** The pet and tool are safe.

| Skill | Primary | Tool | Station | Minigame |
|---|---|---|---|---|
| Manufacturing (land/sea utility tools: AoE stun, poison, EXP-mob summons, ship tools) | Woodcutting (same-level wood is always an ingredient) | Black Hole Crystal | Substance Generator, Shaitan | Stop the bar. The result word sets the yield: Thank you 0 / Abysmal 1 / Bad 2 / Great 3 / Perfect 4 |
| Crafting (wearable equipment) | Mining (same-level ore) | Anti Matter Crystal | Big Furnace, Thundoria | Bet Big or Small on 3d6 (3-10 or 11-18) |
| Cooking (HP/SP restores, timed buffs, short invulnerability) | Fishing (same-level fish) | Crystal Cauldron | Matchsticks in Argent, Thundoria, Shaitan and Icicle | Stop at the green zone, about 75% of the bar. Too early undercooks, too late burns |

[Derived] The crafting dice are a pure coin flip: P(3d6 ≤ 10) = 108/216 = **50%**, and Big has the same 50%. The "choice" is purely cosmetic. How the dice interact with the blueprint's base rate isn't stated. **[Uncertain]**

### Design takeaways
- **Blueprints work as loot rather than unlocks.** Recipes have limited uses and random stats, so they can be traded and gathering demand keeps refreshing. This is a strong design: crafting becomes a market instead of a solo checklist.
- **Every recipe requires a same-level raw material from its gathering skill,** which guarantees steady demand for gatherers at every tier.
- **Minigames that change yield (0-4 items) add a skill element on top of the RNG.** But Crafting's dice "choice" is fake (50/50 either way). Make sure minigame choices actually matter, or players eventually feel cheated.
- **Tool EXP gives 1 per failure as a consolation.** That small comfort, combined with skill levels not adding success, makes the tool the place where progression lives. You could reuse this idea for gem tools.

---

## 13. Offline Stall (player market)

### How it works
- Set up a stall, then log out through Alt+O. The character stays in the world selling.
- It closes automatically when everything has sold, or after **96 hours with no sale**. Each sale resets the 96h timer.

### Design takeaways
- **Offline vending gives you a player market without building an auction house,** and it fills towns, which reads as a busy world.
- **The 96h idle timeout** clears abandoned stalls while keeping active sellers listed. A good default.
- There's no listing fee or sales tax, so the market has **no gold sink**. Add a small listing fee or tax to slow inflation.

---

## 14. Bank and Bank Charms

### How it works
- Bankers in Argent, Shaitan, Icicle, Spring Town and Thundoria all open the **same shared vault**. It can only be used in town.
- **200G per opening.** Base capacity is **48 slots.** Each **Bank Charm (4 Rum)** adds **8 slots**, up to **400**. [Derived] Maxing out takes 44 charms, about **176 Rum.**

### Design takeaways
- **Charging per opening** is a small, frequent gold sink that also discourages using the bank as a second inventory.
- **Storage is the classic gentle monetization:** players see it as convenience, not pay-to-win. A 400-slot cap stops storage from growing without limit.

---

## 15. Item Locking Stone (anti-scam)

### How it works
- **10 Rum each.** A locked item can't be traded, dropped or deleted, and this works on gear, pets and wings.
- **Requires 2FA on the account.** Unlocking uses **a second stone** plus a 2FA code. [Derived] One lock-and-unlock cycle costs 20 Rum.

### Design takeaways
- **Tying item locks to 2FA** makes account security pay for itself and pushes players to enable 2FA.
- Charging for both lock and unlock is a heavy cost. A free lock with a paid or timed unlock would protect more players while still earning money.

---

## 16. Ship System and Sea Travel

### How it works
- **4 builders** (Sinbad in Argent, Attan in Thundoria Harbor, George in Icicle/Atlantis Snow Field, Franklin in Shaitan). Each sells 4-5 hulls, **9 ship types total**, each with a level requirement.
- **Customization happens once, at build time:** you pick bow, engine, cannon and other components. This is also where you add the Fishing Net or Mechanical Arm for sea life skills.
- **Ships have hull (HP) and fuel.** Harbor operators offer **Set sail / Repair / Refuel / Salvage** (salvage revives a ship that was sunk or ran out of fuel). You have to launch from the harbor in the **same city where the ship was built**. Ship-building, repair and refuel prices aren't given. **[Uncertain]**
- **Ship EXP** comes from sailing and from killing sea or land mobs from the sea. Builders level ships up for **gold plus ship EXP**, up to **Lv100**.
- **Trading a ship** wipes its EXP and costs **10 levels** (or half its level at Lv10 or below; the exact rule under Lv20 isn't clear). **[Uncertain]**
- **Uses:** sea-only drops and story bosses, fishing, docking at islands instead of paying teleport fees, and attacking land mobs from the water safely (levelling character and ship together).

### Design takeaways
- **Fuel and hull are running costs** that act as an ongoing gold sink for travel. Sailing also competes with paid teleports, so players choose between paying with time or with gold.
- **The level penalty on traded ships** stops a market for power-levelled ships while still allowing trades. It's a good template for any "levelled asset".
- **Customization only at build time** makes the first build a real decision. It also forces a rebuild for a different role, which is another gold sink.

---

## 17. NPC Services (economic infrastructure)

### How it works
| NPC | Economic role |
|---|---|
| Blacksmith (per city) | Sells low-tier "NPC weapons" for that city's classes. **Smithy** alone handles forging, sockets, fusion and strengthening. |
| Tailor | Sells low-tier armor, gloves and boots for that city's classes. |
| Grocer | Class skill books, life skill books, tickets, inventory locks and materials. **Amos** alone handles gem combining and upgrades and sells Composition Scrolls. |
| Physician | HP/SP potions, revives and cooking ingredients (same stock in every city). |
| Teleporter | Teleports to the other cities and between havens for a fee, and saves your spawn point. |
| Builder / Harbor Operator | Building and upgrading ships, then repair, refuel and salvage. |
| Banker | Shared vault, 200G per opening. |

### Design takeaways
- **Putting each high-value service behind one unique NPC** (Smithy, Amos) turns specific towns into hubs, where stalls cluster.
- **Selling NPC gear by city and class** ties starting regions to classes and gives every class a floor-quality option, which caps how much low-level player-made gear can sell for.

---

## 18. Endgame: Lv85 Weapons and Rings

### How it works
1. **Get a sealed item:** Lv85 rings come from the *Deity Random Sealed Treasure Chest* and weapons from *God's Random Sealed Blessing Bag*. Both drop from Aurora Area and Dark Area bosses (Firm Guard, Fury Mirroring - Kara, Vicious Relic Protector, Morpheus Abyss Demon). The class you get is random.
2. **Unseal** with Researcher Yee (weapons) or Researcher Little Bell (rings) in Winter. Each recipe is **Unsealed Kara Frozen Altar + a specific old endgame item + the sealed item.** For example, the Crusader sword needs Darkness Emissary, the Shield needs **both** Darkness Emissary and Tear of Hardin, and the rings need set pieces such as Hardin's Bind or Asura's Protection.
3. **Unseal the Kara Frozen Altar** (drops from Black Dragon and Abyss Supreme - Kara). It starts with **20 seals** across 4 rune types (Courtesy, Justness, Pity, Courage). Runes drop from Dawn, Sunrise, Dusk and Sunset chests and the same bosses. **Each rune has a 50% chance:** success removes one seal of its type, and failure adds **+1 Cursed Point**.
4. **Remove the curses** with **Winter Dice** (2 Angelic Dice trade for 1 Winter Die). A success removes 1 curse, and a failure costs only the die. The success rate **goes up as curses pile up**:

| Cursed points | Dice success |
|---|---|
| 1-5 | 30% |
| 6-10 | 50% |
| 11-15 | 80% |
| 16+ | 100% |

**[Derived] Expected cost:** 20 seals at 50% means about **40 runes** and about **20 curse points** on average. Clearing 20 curses takes 5×1 + 5/0.8 + 5/0.5 + 5/0.3 ≈ **38 Winter Dice, about 76 Angelic Dice.** The last few curses are the most expensive (about 17 dice for the final 5). That assumes 5 seals per rune type, which the guide doesn't state. **[Uncertain]**

### Design takeaways
- **Old endgame items get consumed as inputs.** Each Lv85 unseal eats the previous tier's best gear, which retires old items from the economy and stops gear-tier inflation. That's very worth copying.
- **Failure has two stages and recovers on its own:** rune failures add curses, and dice get easier as curses grow. Bad luck is bounded and players are never hard-locked. It's an elegant way to soften pure RNG.
- **Getting a random class from the chest** creates natural trading between classes (a sealed Bow is worthless to a Crusader but sellable). This is deliberate liquidity design.
- The many layers (boss → chest → sealed item → altar → runes → dice → old item) give several separate farming targets, which spreads players across content.

---

## Gold faucets and sinks

| Flow | Type | Mechanism | Magnitude (from guides) |
|---|---|---|---|
| Selling mob drops and junk to NPCs | **Faucet** | Main source of new gold | Not stated |
| Quest rewards | Faucet (probable) | Story and life quests | Not stated **[Uncertain]** |
| Player trade and offline stalls | Transfer | Gold changes hands; no tax | None (no sink) |
| Gem combining | **Sink** | 50k + 5k scroll per try; lost on failure | ~2.46M expected per Lv5 gem (no fruit) [Derived] |
| Gem quality upgrade | Sink | 50k + 5k per step | 385k from Broken to Unique |
| Socket creation | Sink (+ Rum) | 50k + 100k, plus 25+25 Rum items | 150k per item |
| Forging fee | **Major sink** | "+X" × 100k, lost on failure | ~2.9M for socket 1 and ~7.0M for socket 2 (expected, no fruit) [Derived] |
| Apparel fusion | Sink | 1k × item level | 40-85k per fusion |
| Apparel upgrade | **Major sink** (+ Rum) | 1.21M-2.25M per try at 50% | ~17.1M expected to reach 110% [Derived] |
| Pliering | Sink (+ Rum) | 10k per gem, plus 15/29 Rum pliers | Small |
| Bank access | Micro-sink | 200G per opening | Small but frequent |
| Life skill books | Sink | 1,000G each (primary/Lv1) | Small |
| Teleport fees | Sink | Fee per jump | Not stated |
| Repairs (gear, tools) | Sink | Repair fees / Repair Tools | Not stated |
| Ship build, upgrade, repair, refuel, salvage | Sink | Gold per service; upgrade costs gold + EXP | Not stated **[Uncertain]** |
| NPC purchases (potions, NPC gear, scrolls) | Sink | Fixed vendor prices | 5k per scroll |
| **Item sinks** (no gold, but destroy value) | Sink | Gems and refs lost on failed combines or forges; refs never refunded on plier; old endgame gear consumed for Lv85 unseal; ship EXP and levels lost on trade; blueprint uses and materials consumed | Very large: ~195 Lv1 gems per Lv5 socket with no fruit [Derived] |
| **Rum (premium) sinks** | Monetization | Stabilizer/Catalyst 25 each, pliers 15/29, Strengthening Crystal 7, Bank Charm 4, Lock Stone 10, Alloy Pickaxe 29, Insurance cards, Fusion Catalyst, apparel | ~50 Rum to socket an endgame item, ~70 Rum to reach 110%, 176 Rum for full bank [Derived] |

**Net read:** PKO has essentially one gold faucet (selling to NPCs) against a very steep set of sinks on the gem, forge and upgrade path. The heaviest drain is the RNG chain on gems, where failures compound. The obvious gap is **no tax on player trade or stalls**, so gold that circulates between players is never removed. For a gem-centred game, the lessons are: (1) make the gem chain your main sink, (2) let players soften the RNG with scarce quest buffs plus premium insurance, but always leave one material at risk, and (3) add a market tax that PKO lacks.

---
Back to [[PKO Teardown]]
