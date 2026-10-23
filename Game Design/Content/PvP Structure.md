---
tags:
  - gdd
  - content
  - pvp
---

# PvP Structure

## PvP Philosophy

PvP is a build showcase and competitive expression system. Balance targets rock-paper-scissors dynamics, not perfect symmetry.

## Reward Boundaries

PvP rewards must not compete with dungeon progression. PvP is a build showcase and competitive expression system, not an alternate gear path.

### PvP Rewards Include:
- PvP tokens (spent on cosmetics and alternate modifier variants)
- Cosmetic unlocks (ranked auras, titles, seasonal armor skins)
- Alignment points (feeds [[Alignment System]] depth)
- Rare modifier gems (alternate acquisition, not exclusive power)

### PvP Rewards Exclude:
- Primary gear drops
- Expedition-tier gems
- Crafting materials in quantities that compete with dungeon farming
- Any item that creates a "must PvP to progress" feeling

PvE players should never feel forced into PvP. PvP players should never feel their rewards are pointless cosmetics. The balance is: PvP rewards are **lateral power and prestige**, not vertical progression.

## Modes

### Arena (1v1)

- Ranked ladder with seasonal rewards
- Build matchmaking: similar power budget builds matched together
- Best of 3 rounds, 2-minute rounds
- No consumables allowed
- Rewards: Ranked currency, cosmetic titles, seasonal auras

### Skirmish (3v3)

- Team-based ranked mode
- Pre-made or matchmaking
- Role diversity encouraged (damage/support/control comp bonuses)
- Single elimination, best of 3 matches
- Rewards: Team rankings, guild reputation

### Battleground (10v10)

- Objective-based (capture points, payload, resource control)
- Casual and ranked queues
- Alignment warfare: Radiant vs Corrupted special events
- Rewards: Honor currency for cosmetics, alignment points

### Challenge Duels (Unranked, Player-Set Rules)

Adapted from PKO's bar PvP, where challengers set the match rules. These are social, showcase and practice fights.

- Issued from any hub to a specific player. Both players must accept the ruleset before the match starts.
- **Rule toggles** (challenger proposes, defender can counter-propose once):

| Toggle | Options |
|---|---|
| Consumables | On / Off |
| Alignment Factor | On / Off |
| Stat normalization | Off / Level 50 baseline |
| Rarity cap | None / Rare / Common-only |
| Modifier slots | Full / 1 per gem / None |
| Commit timer | 6s / 8s / 10s |
| Format | Bo1 / Bo3 / Bo5 |

- No rating, no Honor gain, no alignment points. Combat logs are always saved and shareable as replays.
- **Optional stake:** uses the existing Honor wager-match entry ([[Economy Model]]). Both players pay equal Honor to the Arena NPC, the winner receives 90%, and 10% is burned. The NPC holds the pot; there is no direct transfer ([[Technical Architecture#Constraints (Must Not Violate)]] #5).
- Anti-farming: staked duels between the same two accounts are limited to 3 per day. Repeated asymmetric stake outcomes are flagged.

### Last Tide (Free-for-All Event)

Adapted from PKO's Chaos Argent: **the reward pool is split among survivors.**

- Runs in scheduled [[PvE Structure#Tide Windows (Scheduled Content)|Tide Windows]], 3 per day.
- 20-40 players, solo entry. Guildmates share immunity ([[Guild System#PvP Integration]]), but at most 5 members of one guild per instance.
- 15-minute match. The arena collapses in stages, and defeated players are out.
- A fixed pool of Honor and PvP tokens per instance is split **equally among everyone alive at the end**. Fewer survivors means a bigger share each.
- If everyone truces, everyone gets the minimum share, so betrayal is always individually tempting. That is the intended social dilemma.
- Eliminated players receive small participation Honor.
- Rewards stay inside [[#Reward Boundaries]]: Honor and tokens only, no gear and no gems.

### Naval PvP (Post-MVP)

- Guild ship battles
- 5v5 ship combat with boarding mechanics
- Crew roles: Captain, Gunner, Boarder, Healer, Navigator
- Control trade routes for economic advantage
- Rewards: Guild territory, trade route control, naval cosmetics

## Balance Framework

- Power budget system ensures builds total ~80-100 points across axes
- No build should exceed 60% win rate vs the field
- Monthly balance patches based on data
- "Overperforming" builds get gentle nerfs (reduce outliers, don't gut builds)
- Community build data published monthly (transparency)
- PvP balance is independent of PvE — skills can have separate PvP coefficients if needed to prevent one mode's balance from distorting the other

## PvP Seasons

- 3-month seasons
- Seasonal rewards: Exclusive aura, title, arena cosmetic set
- Rankings reset with soft MMR carry (top players start mid-tier, not bottom)
- Off-season: Experimental mode with modifier restrictions or buffs

## Anti-Grief Systems

- No open-world PvP ganking (PvP is opt-in or zone-based)
- Alignment penalties for repeated same-target killing
- Report system with automated detection
- PvP stat normalization option for casual modes

---

See also: [[Gem System Math]], [[Alignment System]], [[Gem Identity System]]
