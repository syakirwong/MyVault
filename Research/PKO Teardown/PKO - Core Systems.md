---
tags:
  - research
  - pko
  - core
source: https://piratekings.online/guide/
captured: 2026-10-23
---

# PKO - Core Systems

Source: 23 scraped player guides for Pirate King Online (a Tales of Pirates private server). Everything below is my synthesis. Where the guides contradict each other or leave things unclear, I mark it **[UNCERTAIN]**. Numbers come from the guides as written, and I did not check them against the game client.

---

## 1. World and Lore Premise

### How it works
- **Tone:** bright, comical, cartoon 3D with exaggerated animations. Piracy is the theme, set on top of "5000 years" of invented history.
- **Geography:** three macro-regions.
  - **Deep Blue:** three large islands (Spring, Summer, Autumn) and many islets rich in resources, with hostile natives.
  - **Ascaron:** the main continent. It holds the two developed cities, whose own resources are running out, which pushes people out to Deep Blue.
  - **Magic Ocean:** a desert continent with a manufacturing city that worships the goddess Kara.
- **Four hub cities, each tied to class identity:**

| City | Role | Class link |
|---|---|---|
| Argent City | Commerce hub, east coast of Ascaron | Swordsman, Explorer training; PvP bar |
| Shaitan City | Manufacturing/religious, desert south | Herbalist; only forging blacksmith |
| Icicle City | Snowbound ruins, undead | Hunter |
| Thundoria Castle | Navy military base | Endgame quest hubs |

- **Cities are safe zones**, where all combat is banned. Guilds can be joined freely.
- **Backstory:** a race-conflict myth.
  - The Morpheus are an oppressed race the humans call "Demons". By law they are barred from priesthood, nobility and senior army rank, and they cannot own more than 100G.
  - In AD 1023 they rose up after a drunk prince killed Morpheus children. The year became known as "Bloody Red 1023".
  - They were exiled to the Isle of Demons, with its capital Abaddon. By AD 2000 the empire had split into three nations.
- The lore pays off in gameplay: the Rebirth Leo Rune requires a 15-question quiz on story NPCs.

### Design takeaways
- Each hub city sets where you start and which class trainer you see first. This is cheap, clear onboarding geography.
- The "resources ran dry at home, so go to sea" premise justifies the sailing and exploration loop inside the fiction. Copy this kind of motive-driven worldbuilding.
- The backstory tells the conflict from both sides (the imperial record against the Morpheus elders' account). It has more depth than the cute art implies, but the guides show almost no link between it and actual factions or PvP. That is a missed hook.
- Dated: the lore reaches players mostly as text and as quiz trivia. Modern players expect it delivered through play.

---

## 2. Characters (Races/Body Types) and Creation

### How it works
- There are 4 fixed character models. Each acts as a race and body type that **hard-gates which classes you can take**.
- Creation flow:
  1. Set an account security code, used later for actions like deleting a character.
  2. Pick one of the 4 models.
  3. Choose name, hairstyle and face.
  4. Choose a birthplace: Argent, Shaitan or Icicle.

| Character | Archetype | Class breadth |
|---|---|---|
| Lance | Male hero, sword | 3 base classes |
| Carsise | Large muscular male | 1 base class (narrowest) |
| Phyllis | Adult female | 4 base classes (widest) |
| Ami | Small female "elf" | 2 base classes |

### Design takeaways
- Body type as a class gate gives each silhouette a clear meaning: Carsise is always the tank swordsman, and Ami is always a caster. It also cuts down the animation and equipment matrix.
- The cost is uneven freedom: Phyllis gets 5 advanced classes and Carsise gets 1. Players who pick the wrong model at level 1 find out at level 10.
- Thin customization (hair and face only) is dated. Fashion and cash-shop apparel carry the self-expression instead.
- Birthplace choice matters little because class trainers are spread across cities anyway. It adds friction for no real benefit.

---

## 3. Classes and Advancement Tree

### How it works
- **1st promotion at Lv10:** a short fetch/kill/deliver quest chain, started from Newbie Guide Senna. Kill counts are small (2 to 12 mobs, 2 to 3 drop items). Each class gets a starter weapon and a skill as its reward.
- **2nd promotion at Lv40:** one quest per class at the class NPC, with heavier gathering.
- No 3rd tier exists. Further progression comes from Rebirth (section 8).

| Base class (Lv10) | 2nd class (Lv40) | Races allowed (2nd class) | 2nd-class quest NPC | 2nd-class requirements (summary) | Role |
|---|---|---|---|---|---|
| Swordsman | **Crusader** | Lance, Phyllis **[UNCERTAIN: the advancement guide says Lance only, the Crusader guide says Lance + Phyllis]** | Castle Guard Peter (Argent) | 3 Warrior Certificate, 5 Greasy Lizard Skin, 1 Breast Plate (buy or drop), kill 10 Skeletal Warrior | Dual-sword melee/ranged burst; stun and stealth |
| Swordsman | **Champion** | Carsise only | Castle Guard Peter | 1 Ninja Mask, 5 Great Bear Tooth, 5 Solid Rock, 1 Criss Sword (drop, or 83,593 gold at the blacksmith) | Two-handed sword tank/hybrid |
| Hunter | **Sharpshooter** | Lance, Phyllis | Icicle Swordsman Ray | 5 Old Quiver, 5 Broken Ninja Sword, kill 10 Skeletal Archer and 10 Ninja Mole | Only ranged physical class; bow or gun; seals |
| Herbalist | **Cleric** | Phyllis, Ami | High Priest Gannon (Shaitan) | 6 Elven Fruit Juice (drop, or craft from 10 fruit + 1 glass), 4 Panacea, 2 Fancy Petal, split across 2 NPCs in different towns | Magic damage, heal, revive, SP shield |
| Herbalist | **Seal Master** | Phyllis, Ami | High Priest Gannon | 3 Dingle Bell, 10 Dangerous Sharp Claw, 10 Heart of Purity, then turn in to Holy Priestess Ada | Control/debuff support |
| Explorer | **Voyager** | Lance, Phyllis, Ami | Little Daniel (Argent) | Talk to 4 NPCs on 4 sea islands in any order; one is reachable only by ship | Coral-powered AoE magic; strong at sea |

**1st-class access by character:**

| Character | Swordsman | Hunter | Herbalist | Explorer |
|---|---|---|---|---|
| Lance | Y | Y | - | Y |
| Carsise | Y | - | - | - |
| Phyllis | Y | Y | Y | Y |
| Ami | - | - | Y | Y |

The advancement guide's line for Phyllis is garbled ("Herbalist, Sharpshooter or Explorer Swordsman"). I resolved it with the Characters page, which lists Hunter, Swordsman, Herbalist and Explorer.

1st-promotion rewards:

| Class | Weapon | Skill |
|---|---|---|
| Swordsman | Short Sword | Concentration |
| Hunter | Short Bow | Range Mastery |
| Herbalist | Mystic Branch | Heal |
| Explorer | Dagger | Diligence |

**Per-class profile (from the class guides):**

| Class | Primary / secondary stat | Signature skills | Weak phase | Strong phase |
|---|---|---|---|---|
| Crusader | STR / AGI | Illusion Slash (ranged, scales with attack speed), Berserk (attack-speed buff), Shadow Slash (stun), Stealth | Lv<40 skill tree is poor | Late game; easy solo leveling |
| Champion | CON / STR | Illusion Slash, Blood Bull (passive HP/DEF), Primal Rage | Early game, solo | Late game; boss tanking |
| Sharpshooter | ACC / AGI (CON scales worst of any class) | Firegun Mastery, Cripple (dodge down + slow), Enfeeble (silence + attack down) | Mid game | Late game; best bosser; fastest mover |
| Voyager | SPR / CON (often a 50/50 split) | Lightning Bolt, Conch Ray (AoE line, low cooldown), Tornado (stun) | Early game (one damage skill) | Mid game; at sea |
| Seal Master | CON / SPR | Spiritual Bolt, Cursed Fire (DEF down), Abyss Mire (slow), Shadow Insignia (blocks melee), Seal of Elder (silence) | Early game, very party-dependent | Late game; boss and PvP control |
| Cleric | SPR / CON | Spiritual Bolt, Heal, Revival (costs a Revival Clover), Energy Shield (damage drains SP before HP) | Early game, solo | Late game |

**Voyager coral mechanic:** every active skill needs a matching coral (Thunder, Strike or Wind) equipped. Each cast uses 8 coral energy; for example, a Lv2 coral holds 900 energy. Corals replace ring and necklace slots, which costs the Voyager stats.

**Gear progression the guides name:**

| Stage | Gear |
|---|---|
| Lv35-45 | Unsealed sets |
| Mid game | Chest sets (Inca, Enigma, Eva) |
| Lv55 | Chaos Argent sets and Lv55 unsealed weapons |
| Lv70 | Boss stones / "frames" |
| Lv75 endgame | Black Dragon set |

Different classes value different tiers.

**Level-capped mazes as balance checkpoints:**
- Forsaken City (max Lv45)
- Dark Swamp (Lv55 gear tier)

The class guides grade every class at both, so the community treats these caps as fixed PvP/PvE brackets.

### Design takeaways
- The tree is shallow but legible: 4 → 6 classes over two gates (Lv10, Lv40). Each 2nd class has a clear verb: stun, tank, snipe, AoE, seal, shield. Easy to balance and easy to explain.
- Promotion quests are small, themed errands that send players across the starter zones. They teach the map as a side effect. The Voyager quest forces a first sea voyage, a neat way to teach the sailing system.
- Every class has a stated weak phase (early, mid or late). Uneven power curves push grouping (herbalists rely on parties) but frustrate solo players. Decide on purpose which classes are allowed to be solo-weak.
- Level-capped mazes create stable brackets where a character "frozen" at a level stays relevant. This is a strong, cheap retention structure worth copying.

---

## 4. Stats

### How it works
- **5 allocatable base stats**, capped at **100 each**.

| Stat | Effect |
|---|---|
| STR | Melee attack |
| AGI | Attack speed, dodge |
| SPR | Max SP, SP regen, magic damage and healing |
| CON | Max HP, HP regen, defence |
| ACC | Hit rate, ranged attack |

- **Derived stats**:
  - Visible: HP, SP, Min/Max Attack, Hit Rate, Dodge, Defence, Attack Speed, Physical Resist.
  - Hidden: Movement Speed, Crit Rate, HP regen, SP regen.
- **Two mitigation layers:**
  - **Defence** = flat reduction.
  - **Physical Resist** = percentage reduction.
- **Hit/dodge** is a contested pair (Hit Rate against Dodge).
- **Stat point income:**
  - 4 at Lv1.
  - 1 per level.
  - 5 on every 10th level.
  - 2 per level from Lv60 onward.
- **Scaling rules (qualitative only, no formulas published):**
  - Returns are "exponential", so more points in one stat give a bigger effect per point.
  - Bonuses arrive in steps every few points.
  - Scaling differs by class. Sharpshooter has the worst CON scaling.
- Only Unique Gems grant base stats. Other gems, passives and buffs raise derived stats.

**My estimate of total stat points by level.** I read "5 per 10th level" as replacing the normal 1 on that level. Both the income reading and the level cap are **[UNCERTAIN]**; the guides mention Lv85 in event brackets.

| Level | Approx. cumulative points |
|---|---|
| 40 | ~59 |
| 59 | ~82 |
| 80 | ~133 |

Against a ceiling of 500 (5 × 100), a Lv80 character can fill about 27% of it. In practice that means one stat near its cap plus a minor second, which matches the "full damage / full tank / hybrid" builds the class guides describe.

### Design takeaways
- Increasing returns plus scarce points force specialization. That makes builds feel meaningful, but it also makes hybrids weak and mistakes costly. Rebirth's full stat reset is the safety valve.
- The cleanest part of the design is each stat having one obvious job, with an exception per class (AGI only adds damage on Sharpshooter, and boosts Illusion Slash on Crusader).
- Dated: hidden stats (crit, movement speed) and unpublished formulas push players to community spreadsheets. Show breakpoints in the UI.
- The doubled income at Lv60+ is a good late-game acceleration. It makes the slow late levels still feel like they move the character.

---

## 5. Skills

### How it works
- Each class learns skills from **Skill Books** bought at class-city grocers. Books are colour-coded: Swordsman and Explorer orange/light-blue (Argent), Herbalist green (Shaitan), Hunter yellow (Icicle). Double-click a book to learn it.
- **Max skill level is 10.** Many skills need another skill at a set level first, which forms a tree.
- **Skill points:**
  - 1 per level from Lv10.
  - 1 extra every 5 levels from Lv65.
  - That comes to about 75 points by Lv80 **[UNCERTAIN, same caveat as stat points]**. It is too few to max everything, by design.
- **Active vs passive:**
  - Actives cost SP and have cooldowns. They are used from the skill window or the F1-F12 hotbar.
  - Passives are mostly permanent stat boosts, sometimes conditional.
- **Three skill tabs:**
  - **Combat** (sword icon).
  - **Life skills** (axe icon): woodcutting, mining, salvage, fishing, and cooking implied by the Rebirth wine tasks. They use a separate **life skill point** currency, which comes almost only from Story Quests.
  - **Fairy skills:** need an active Fairy pet and use the fairy's stamina instead of SP. They come from special books and cannot be levelled, except Fairy Possession.

### Design takeaways
- Two separate point currencies (combat and life) keep gathering from competing with combat power. It also gives the long Story Quest chain a concrete payoff. Good decoupling.
- Scarce points plus prerequisite chains make planning matter, but there is no respec outside Rebirth. That creates "dead" characters. Add a paid or limited respec.
- Fairy skills spending pet stamina is a clever way to hang a second resource bar on the pet system, and it ties pets into combat.
- Buying books at a vendor is dated friction. It does create a gold sink and gives players a reason to visit cities.

---

## 6. Levelling, Experience and Monsters/Combat

### How it works
- **Combat:**
  - Click-to-attack auto-attack plus skills.
  - Holding Shift shows the HP, name and level of every nearby mob.
  - Mobs show as red dots on the minimap.
- **Monster aggro behaviour, in 3 classes:**
  - **Passive:** never fights back.
  - **Retaliatory:** fights only when hit, and gives up past its chase range.
  - **Aggressive:** attacks on entering its aggro range. All bosses and mini-bosses are aggressive.
- **Two monster-only stats:**
  - **Aggravation range:** detection radius.
  - **Chase range:** leash distance, after which the mob resets home.
- **Monster tiers:**
  - Normal.
  - Mini-boss: a stat-boosted version of an existing mob.
  - Boss: needs a team and is partly immune to crowd control (CC). Bosses mostly live in mazes and dungeons.
  - Treasure Chest: a "monster" that never attacks and has 0 dodge, guarded by mob packs.
- **Land vs sea:** sea mobs drop valuable crafting and quest materials but level you slowly. Land mobs die faster for the same XP.
- **XP rules:**
  - Best XP comes from mobs ranging from 9 levels above the player down to 4 levels below.
  - When several players hit one mob, damage share decides the XP split, but whoever pulled aggro gets a large share.
  - Healing a player threatened by a mob moves that mob's aggro to the healer.
- **Class/mob fit:** high DEF/PR mobs suit magic classes. Low-dodge, high-HP mobs suit physical classes.
- **Drops:** 1-2 common junk drops (vendor gold, quest items) and rare equipment.

### Design takeaways
- The passive/retaliatory/aggressive split plus aggro and chase ranges is a minimal, readable AI kit. That is all a 2000s-era MMO needs, and it still holds up for open-world grinding.
- The XP window favours mobs above your level, which rewards fighting up, unlike games that penalise it. Pair that with the hit-and-run kiting the class guides describe.
- Heal-transfers-aggro is an elegant implicit tank/healer lesson. The damage-plus-tagger XP split is dated and invites kill-stealing arguments.
- The land/sea split lets sea content carry economic value without being the levelling path. It gives two loops for two motives.

---

## 7. Quests

### How it works
- **Markers:**

| Mark | Meaning |
|---|---|
| Green ! | Quest available |
| White ! | Quest in progress |
| Yellow ! | Ready to turn in |

- **Quest log:** Alt+Q, maximum **10 active quests**. Abandoning wipes all progress.
- **Three task verbs** (often mixed in one quest): kill X, gather X, talk to or deliver to an NPC.
- **Special categories:**
  - **Story Quests:** 100+ strictly sequential quests. They give gold and XP and are almost the only source of life skill points.
  - **Class Promotion:** the only mandatory quests.
  - **Class Life quests:** after 1st promotion, a repeatable cycle of randomised objectives. A streak without abandoning raises rewards, and the last quest in a cycle gives a large bonus.
- **Language Barrier** is an example of a gate quest.
  - Spring Town NPCs cannot be understood until you finish it.
  - Costs: 100,000 gold for a prescription, 90 fish bones from 3 sea mob types, 10 jelly skins, and a further 50,000 gold.
  - It unlocks the town's quests and trades, an Alloy Pickaxe, a Shooting Star and life skill points.
  - The Water Wheel story chain grants the same unlock automatically, so the two overlap.
- **Hexathlon** (a weekend event):
  - Six 30-minute start windows: 06:00, 12:00 and 18:00 on Saturday and Sunday.
  - Open to Lv85 and below.
  - 6 tasks: deliver a letter; hand in a Super Candy Stick (cash shop, 9 rum for 3); kill mobs scaled to your level bracket; deliver 3 Wood, 3 Ore and 3 Sashimi (one gathering skill each); hand in 5 Elven Signets (passive pet drops); and finish in a 3-person party with one member each in the Lv20-30, Lv31-40 and Lv41+ brackets.
  - Only the first task must begin inside a window, and runs repeat back to back.

Hexathlon XP per completed run, as a percentage of the current level's XP bar:

| Level | XP per run |
|---|---|
| 11-20 | 800% |
| 21-30 | 600% |
| 31-40 | 400% |
| 41-50 | 200% |
| 51-60 | 60% |
| 61-64 | 33% |
| 65-70 | 13% |
| 71-75 | 3.12% |
| 76-80 | 2.08% |
| 80-85 | 1.39% |

### Design takeaways
- Hexathlon is the standout design here. One run touches delivery, the cash shop, combat, all three gathering skills, the pet system and a party requirement that **forces mixing level brackets**. A weekly ritual like this teaches systems and sets up mentoring through play. The percentage-of-level reward stays meaningful at every tier, and a cheap paid item sits in the loop.
- Cycle quests with streak bonuses are a good low-cost repeatable. The streak punishes abandoning without blocking it.
- Gating a town's language behind a quest is a memorable piece of world-as-lock design. Having a second unlock path (Water Wheel) for the same gate is redundant.
- Dated: a 10-quest cap, abandon-wipes-progress, and 100+ strictly linear story quests hiding a needed resource (life skill points).

---

## 8. Rebirth (1st and 2nd)

### How it works

**1st Rebirth ("Phoenix Rebirth")**
- **Gate:** Lv75+ and 9,999 Reputation.
- **Access:** the quest is taken from the Goddess in Heaven. Reaching Heaven takes either a Goddess's Favor or a 4-item boss-drop set (Bone of Mummy, Soul of Corpse Soldier, Heart of Pharaoh, Holy Bible).
- **Goal:** collect **8 Runes** for a Rebirth Stone.

| Rune(s) | Task |
|---|---|
| Leo | 15-question lore and mechanics quiz, then kill 15 each of 6 mob types across Lone Tower floors 1-6 |
| Roico | Needs Language Barrier done. Gather 30 each of 3 materials, then bring cooked wines (life skill cooking Lv5-6): 50 Mao, 20 Dukan, 15 Ginseng |
| Baby | First group content: kill 3 world/sea bosses (Deathsoul Commander, Barborosa, Prehistoric Giant Octopus "Kraken") |
| Remaining 5 (CiMen, Dina, Bragi, Mel, Amon) | **Player chooses a path** (see below) |

The path choice for the last 5 runes:
- **Tough path:** a single quest to kill Wandering Soul, Snowman Warlord and Black Dragon, which are maze bosses.
- **Troublesome path:** a long solo-friendly grind.
  - **Timed sea race (Challenge Genesis).** Visit every sea haven three times: once in under 30 minutes, once finishing between 7 and 8 hours, and once finishing between 5 and 5.5 hours.
  - **Aries gear check.** Wear a full Aries set from Captain difficulty and hand in a Seal of Aries.
  - **World tour.** Visit 12 NPCs at 500 Reputation each, about 6,000 Rep (the guide says 6,500 **[UNCERTAIN]**).
  - **Click grind.** Use candles 9 then 99 times (10 s cooldown each), then a statue 999 times (30 s cooldown), about 8.3 hours of clicking at minimum.
  - **Low-level mob kills.** Kill 110 each of 6 low-level mob types, 660 kills in total.

Switching paths costs 10,000,000 gold.

**To redeem the stone:** unequip everything, free 6 inventory slots, and choose a class. The choice is irreversible, and the class must be legal for your character model.

**1st Rebirth rewards:**
- Class change.
- Full stat and skill reset.
- Passive **Rebirth Mystic Power**: +5.5% physical damage dealt and -5.5% physical damage taken.
- A class-specific AoE active skill.
- Cosmetic flying wings.

| Class | Rebirth active skill |
|---|---|
| Crusader | Ethereal Slash: point-blank AoE |
| Sharpshooter | Red Thunder Cannon: line AoE |
| Champion | Beast Legion Smash: AoE + slow |
| Seal Master | Devil Curse: damage over time + debuff |
| Cleric | Holy Judgment: AoE damage + AoE heal |
| Voyager | Super Consciousness: area damage over time |

**2nd Rebirth**
- **Gate:** 1st Rebirth done.
- **Errands:**
  - Pay 1M gold.
  - Gather 100 Snowy White Plume and 100 Black Feather.
  - 5 Argent City errands in any order. One uses a 5-rum cash-shop Rose, and the NPC it is handed to depends on character gender.
  - Kill 3 dragons.
- **SEXP grind:** reach exactly 9,890 "SEXP" from repeatable boss-challenge quests. These are taken one at a time, worth 50-120 SEXP each; the Black Dragon is worth 120. That is about 83-198 boss kills. The guide warns that going past 10,000 breaks the quest.
- **Abaddon bosses:** kill 12 bosses in order on Abaddon floors 5-17 (Revival I and II, 6 runes each).
- **Rewards:** class change, stat and skill reset, "small power boost" **[UNCERTAIN size]**, glowing wings.

### Design takeaways
- Rebirth works as a prestige reset and as a respec in one. It fixes the scarce-points problem from sections 4-5 while keeping a permanent gain: +5.5% damage and mitigation plus a new AoE. This is the core long-term hook.
- Each rune demands a different skill: lore knowledge, life skill cooking, group bossing, sailing. That turns endgame into a test of the whole game. The Tough/Troublesome split lets group players and solo players reach the same goal.
- Dated and hostile parts: 999 statue clicks, exact time windows (5-5.5 hours), an SEXP overflow bug, a 10M switch fee, a hard-to-find 15-answer quiz. These are padding, not challenge. Keep the "tour of every system" structure and drop the clicking.
- The visual wings payoff is a strong status signal for little effort. Prestige cosmetics clearly drive this grind.

---

## 9. PvP

### How it works
- **Location:** consensual PvP only, run from the Argent City Bar. Cities are otherwise safe.
- **Duel ticket:** Lv25+ players buy a **Medal of Valor** for 50,000 gold. It is bound to the account forever and cannot be deleted. It tracks Honor, Kills, Deaths, Battles and Victories, and also shows unrelated Party Contribution and Chaos Points.
- **Starting a match:** right-click a player and choose PvP. For party fights, leader invites leader and every member must accept.
- **Modes:**
  - **Party PK:** 1v1 up to 5v5, stakes Honor and stats.
  - **Free PK:** no Honor risk, and players with negative Honor can still join.
- **Gold wager:** up to 2,000,000,000 gold, with an extra confirmation step.
- **Rule toggles, set per match** (8 rule toggles, 9 bans in total since the buff toggle also strips existing buffs):
  - stealth
  - debuff removal
  - buffs (strips existing ones and blocks new ones)
  - potions
  - revives
  - basic attacks
  - all skills
  - crafted consumables such as flash bombs
- Arena map: one standard map plus seasonal maps.
- **Penalties:** logging out or leaving mid-match counts as a loss. At -300 Honor you are locked out of ranked MvM until you buy a Book of Honor Bonus (149 rum, +100 Honor).
- The UI mentions a "Camp" chat channel for "Sacred War" factions. That points to a faction war mode the guides do not cover **[UNCERTAIN]**.

### Design takeaways
- Per-match rule toggles are a strong, cheap feature. Players can run "no stealth, no potions" duels that remove the counters they dislike, which works as community-run balance. Copy this.
- Consensual bar PvP with wagers keeps open-world griefing away while still feeding a gambling economy and a gold sink (the 50k medal).
- Selling Honor recovery in the cash shop is pay-to-escape-penalty. It cheapens the ranking and is a monetisation pattern to avoid.
- An account-bound, undeletable medal used as a permanent PvP record gives a visible reputation. But showing unrelated stats on it (Chaos points, party contribution) muddies the meaning.

---

## 10. Mentorship (Mentor and Disciple)

### How it works
- **Eligibility:** disciple must be Lv40 or below; mentor must be Lv41 or above.
- **Limits:** one mentor per disciple, and up to 4 ungraduated disciples per mentor.
- **Linking:** right-click the other player, choose Mentor or Disciple, and they confirm.
- **Rewards are automatic on disciple level-ups**, whether or not the mentor actually helped.
  - **Mentor:** a small amount of Reputation per disciple level, plus 600 Rep when the disciple graduates at Lv41.
  - **Disciple:** 300 Rep, 100,000 gold and 49 Fairy Coins on graduation, plus one Mini Amplifier of Strive per level (+100% XP for 200 s, usable below Lv40).
- **Breaking the bond:**
  - Mentor removing a disciple loses Reputation.
  - Disciple removing a mentor loses gold.
- **Tie-in to Rebirth:** mentor Reputation feeds the 9,999 Rep Rebirth gate.

### Design takeaways
- Rewarding the mentor whether or not they helped makes the system easy to farm: collect 4 disciples and wait. It still pays for the social link up front, and Rep feeding Rebirth gives veterans a real reason to recruit.
- Penalties on both sides for breaking the bond discourage casual churn.
- A per-level XP booster for the disciple is better than a lump sum because it keeps paying through the whole Lv1-40 stretch.
- Fix: weight mentor rewards by real co-play (party time, shared kills) to stop idle farming.

---

## 11. UI and Controls

### How it works
- **Static HUD:**
  - Player frame: portrait, level, HP/SP/EXP bars.
  - Chat: system feed above player chat.
  - Hotbar: F1-F12, with extra bars that can be paged.
  - Minimap: red mobs, blue players, orange resource nodes, region name and coordinates.
- **Chat channels and prefixes:**

| Channel | Prefix | Note |
|---|---|---|
| Party | ! | |
| Guild | % | |
| Trade | ^ | Costs gold per message |
| World | * | Costs gold per message |
| Camp (faction) | \| | |
| Private message | @name | |

  Chat also has emotes, character actions, and mood icons ("Squidies") shown above the head.
- **Window hotkeys (Alt+key):**

| Window | Key |
|---|---|
| Stats | A |
| Skills | S |
| Quest log | Q |
| Inventory | E |
| Temporary Bag (cash-shop deliveries) | D |
| Guild | C |
| Friends | F |
| Settings | O |
| Help | H |
| Minimap toggle | M |
| Radar | R |
| World map | W |

- **Radar:** type in coordinates and region, and an arrow points the way. The world map shows explored areas only.
- **Performance controls:** Ctrl+B hides player models, Ctrl+S hides stall titles (for crowded areas). Alt+click a player to auto-follow.
- The inventory has a security lock and a trash-can drag-to-delete.

### Design takeaways
- Paying gold to post in global chat is a simple, effective spam limit and gold sink. Copy it.
- The coordinate Radar shows that the game's navigation (guides, quests) runs on raw coordinates, which is dated. Modern players expect quest waypoints and auto-pathing. The Radar is a patch over missing quest tracking.
- Hide-models and hide-stall-titles toggles admit that player crowds and stalls hurt performance. Plan instancing or level-of-detail early instead of relying on client toggles.
- Alt+letter for every window clashes with OS shortcuts and is less standard than the I/C/K/L of later MMOs. Single-key, rebindable shortcuts are better.

---

> [!info] Resolved by code
> The exact stat formulas and stat-point income are reconstructed from the portal's simulator in [[PKO - Stat Formulas]]. That supersedes the guide-based estimates below: 166 stat points at Lv80 including starting stats, 190 at Lv90, and the cap observed on the rank ladder is **Lv90**.

## Cross-system Notes for a Designer

| Lever | PKO value | Effect |
|---|---|---|
| Class gates | Lv10, Lv40, Rebirth at Lv75 | Three clear motivation spikes |
| Point scarcity | ~133 stat pts against a 500 cap; ~75 skill pts by Lv80 (est.) | Forces builds; Rebirth needed as respec |
| Rebirth permanent gain | +5.5% damage dealt, -5.5% taken, new AoE | Small number, large perceived value |
| Reputation | 9,999 needed for Rebirth; earned via mentoring and other sources | Links social systems to endgame |
| Gold sinks | 50k PvP medal, 100k+50k Language Barrier, 10M path switch, 1M for 2nd Rebirth, paid global chat | Inflation control |
| Cash-shop touches | Candy (Hexathlon), Rose (2nd Rebirth), Honor book, travel passes | Small convenience and penalty-escape items; the Honor book is the risky one |

Open questions the source docs do not settle:
- the level cap
- exact stat scaling formulas
- the exact stat-point income reading
- the 2nd Rebirth's power gain
- what the Sacred War faction mode is
- the Crusader race gate

---
Back to [[PKO Teardown]]
