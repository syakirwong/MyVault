---
tags:
  - gdd
  - social
  - guild
---

# Guild System

## Design Philosophy

Guilds are **organizational infrastructure**, not power multipliers. A guild provides community, coordination, identity, and long-term goals -- but never raw combat advantage that unguilded players cannot match. The best guild in the game wins because its members are skilled and coordinated, not because guild bonuses made them statistically superior.

Guilds serve three purposes:

1. **Social anchor** -- a persistent community within the community, with shared identity and goals
2. **Coordination layer** -- group content (dungeons, wars, raids) benefits from guild organization but does not require it
3. **Prestige vehicle** -- guild achievements, territory, and reputation are visible markers of collective effort

---

## Guild Creation

### Requirements

| Requirement | Detail |
|---|---|
| **Minimum Level** | Character level 15+ |
| **Gold Cost** | 5,000 gold (one-time, non-refundable) |
| **Cooldown** | Cannot create a new guild within 7 days of disbanding one |

### Naming Rules

| Rule | Detail |
|---|---|
| Length | 3-24 characters |
| Allowed characters | Letters, numbers, spaces, hyphens, apostrophes |
| Profanity filter | Automated + manual review queue |
| Uniqueness | Server-unique, case-insensitive |
| Rename cost | 2,500 gold (once per 30 days) |

### Guild Tag

- 2-4 character abbreviation displayed alongside member names (e.g., `[STM]`)
- Must be server-unique
- Same character restrictions as guild name (letters, numbers only for tags)
- Displayed as `[TAG] PlayerName` in all UI contexts: chat, combat logs, hub player lists, leaderboards
- Tag change cost: 1,000 gold

### Guild Description

- Up to 500 characters
- Displayed on recruitment board, guild finder, and guild tag tooltip
- Editable by Leader and Officers
- Supports basic formatting and build code references (e.g., "This week's raid build: `DUE-STM-CRT-PHZ-COR`")

### Faction Alignment Affiliation

Guilds may declare **one major faction affiliation** from the [[Factions System]]:

| Faction | Alignment |
|---|---|
| The Luminari | Radiant |
| The Hollow Court | Corrupted |
| The Driftborn | Unbound |

**Affiliation is optional.** Unaffiliated guilds can exist but cannot access faction-specific guild features (see [[Factions System#Faction & Guild Interaction]] for details):

| Feature | Requirement | Effect |
|---|---|---|
| **Faction Guild Banner** | Any affiliation declared | Guild banner displays faction colors and sigil |
| **Faction Rally** | 10+ members at Trusted+ faction rep | +3% all stats for guild members in faction-aligned zones |
| **Faction Quartermaster** | 5+ members at Honored+ faction rep | Guild hall gains faction vendor with discounted faction recipes |
| **Faction Champion Slot** | Guild leader at Exalted faction rep | Guild gains 1 exclusive "Faction Champion" title for top PvP player |

**Switching affiliation:** Requires Leader action + 72-hour cooldown. All faction-specific guild features are lost and must be re-earned with the new faction. Members are notified but not forced to leave.

**Mixed-alignment guilds are viable.** Members of any personal alignment can join any guild regardless of its faction affiliation. Affiliation reflects the guild's political identity, not a membership gate.

---

## Ranks & Permissions

### Default Rank Structure

| Rank | Count Limit | Description |
|---|---|---|
| **Leader** | 1 | Full control. Cannot be kicked. Formerly "Founder." |
| **Officer** | Up to 10% of member cap (minimum 2) | Trusted leadership. Most administrative powers. |
| **Veteran** | Unlimited (within cap) | Experienced members with expanded privileges. |
| **Member** | Unlimited (within cap) | Standard rank. Core participation rights. |
| **Recruit** | Unlimited (within cap) | Probationary. Limited permissions. 7-day default duration before auto-promotion to Member (configurable by Leader). |

### Custom Ranks

- Leader can create up to **4 additional custom ranks** inserted between Officer and Recruit
- Custom ranks have custom names and individually assigned permissions from the permission matrix
- Custom ranks are ordered in a hierarchy set by the Leader
- Custom rank creation unlocked at guild level 5

### Permission Matrix

| Permission | Leader | Officer | Veteran | Member | Recruit |
|---|---|---|---|---|---|
| Invite players | Yes | Yes | Yes | No | No |
| Kick members (lower rank only) | Yes | Yes | No | No | No |
| Promote (up to one below own rank) | Yes | Yes | No | No | No |
| Demote (lower rank only) | Yes | Yes | No | No | No |
| Guild bank deposit | Yes | Yes | Yes | Yes | Yes |
| Guild bank withdraw | Unlimited | Tier 2 | Tier 1 | Tier 1 | No |
| View bank transaction log | Yes | Yes | No | No | No |
| Declare guild war | Yes | Configurable | No | No | No |
| Edit guild description / MOTD | Yes | Yes | No | No | No |
| Change guild settings | Yes | No | No | No | No |
| Set faction affiliation | Yes | No | No | No | No |
| Manage custom ranks | Yes | No | No | No | No |
| Post guild announcements | Yes | Yes | No | No | No |
| Read guild announcements | Yes | Yes | Yes | Yes | Yes |
| Territory bid | Yes | Configurable | No | No | No |
| Guild hall decoration | Yes | Yes | Configurable | No | No |
| War vote participation | Yes | Yes | Configurable | No | No |

**"Configurable"** permissions are toggleable by the Leader per rank or per individual Officer.

---

## Guild Size

### Member Cap Scaling

Guild size scales with guild level. The member cap increases as the guild progresses through collective activity.

| Guild Level | Member Cap | Tier Name |
|---|---|---|
| 1 (creation) | 20 | Band |
| 3 | 25 | Band |
| 5 | 30 | Company |
| 7 | 40 | Company |
| 10 | 50 | Company |
| 15 | 75 | Order |
| 20 | 100 | Order |
| 30 | 125 | Legion |
| 40 | 140 | Legion |
| 50 (max) | 150 | Legion |

**Maximum cap: 150 members.** No guild may exceed this regardless of bonuses or exceptions.

### Active vs Inactive Tracking

| Status | Definition | Upkeep Impact |
|---|---|---|
| **Active** | Logged in within the last 14 days | Counts toward upkeep cost |
| **Inactive** | No login for 14-60 days | Counts toward member cap but NOT upkeep cost |
| **Dormant** | No login for 60+ days | Counts toward member cap but NOT upkeep cost |

- All members regardless of status count toward the member cap
- Officers can view a roster panel with: last-login timestamp, status indicator, primary build archetype, alignment, faction reputation, gem mastery highlights
- Officer notes per member (private, visible to Officers+ only)
- Leader receives a weekly summary: active count, newly inactive, newly dormant
- Leader can set a custom inactivity threshold for flagging (default: 30 days)

---

## Guild Progression

### Guild XP Sources

Guild XP is earned passively through member activities. Individual contributions are tracked and visible to Officers. Guild XP is permanent -- guilds never lose levels.

| Activity | Guild XP | Notes |
|---|---|---|
| Dungeon completion (Normal) | 50 | Per member, per clear |
| Dungeon completion (Challenge) | 100 | Per member, per clear |
| Dungeon completion (High-Risk) | 150 | Per member, per clear |
| Mythic dungeon clear | 200 | Per member, per clear |
| Solo Trial completion | 30 | Per member, per clear |
| World boss kill | 200 | Per participating guild member |
| PvP Arena win (ranked) | 75 | Per member |
| Skirmish win (3v3 ranked) | 100 | Per member |
| Battleground win (10v10) | 150 | Per member |
| Guild war victory | 500 | Collective |
| Guild quest completion | 100-300 | Weekly |
| Member discovers new resonance | 100 | Per discovery |
| Member reaches gem mastery 10 | 150 | Per gem |
| Member reaches Exalted faction rep | 300 | Per member, per faction |

**Daily guild XP cap per member: 2,000.** Prevents mega-guilds from leveling disproportionately fast through sheer headcount. Encourages broad participation over individual grinding.

### Guild Levels and Perks

| Level | Cumulative XP | Perk Unlocked |
|---|---|---|
| 1 | 0 | Guild created: chat, tag, basic roster |
| 5 | 10,000 | Guild bank (50 shared slots), custom ranks unlocked, member cap 30 |
| 10 | 30,000 | Recruitment board priority listing, guild log, member cap 50 |
| 15 | 60,000 | Guild Wars eligibility, war council chat, member cap 75 |
| 20 | 100,000 | Guild hall (Post-MVP), expanded bank (100 slots), member cap 100, territory bidding rights |
| 25 | 150,000 | Guild hall customization tier 2, guild emblem upgrades |
| 30 | 200,000 | Guild leaderboard eligibility, expanded bank (150 slots), member cap 125 |
| 40 | 350,000 | Guild hall customization tier 3, legacy cosmetics, member cap 140 |
| 50 (cap) | 500,000 | **Legacy Guild** status: permanent guild history entry, exclusive emblem border, member cap 150 |

### Guild Quests

Weekly objectives that give the guild a shared focus. 3 quests available per week, guild must choose 2.

| Category | Example | Reward |
|---|---|---|
| **PvE** | "Clear Stormspire with 3+ guild members, Challenge Mode" | 300 Guild XP + 500 gold to treasury |
| **PvP** | "Win 10 Arena matches as a guild (any member)" | 200 Guild XP + 300 gold to treasury |
| **Domain Mastery** | "Trigger 5 unique resonances in guild parties this week" | 250 Guild XP + 400 gold to treasury |
| **Crafting** | "Guild members collectively craft 10 Rare+ gems" | 200 Guild XP + crafting materials to guild bank |
| **Exploration** | "Guild members discover 3 hidden areas across any region" | 150 Guild XP + cosmetic guild marker |
| **Faction** | "Earn 5,000 combined faction reputation this week" | 300 Guild XP + faction-specific guild cosmetic |

Quest difficulty and rewards scale with guild size tier. Band guilds get easier objectives with proportionally adjusted rewards.

### Guild Emblem

- Choose from a library of **symbols** (50+ at launch) and **color schemes** (tied to alignment and domain themes)
- Emblem displays on: guild tag tooltip, guild hall entrance, Guild Wars banners, recruitment board listing
- Legendary-tier cosmetic emblems unlockable through guild achievements
- Emblem change cost: 1,000 gold (prevents constant swapping)

---

## Guild Bank

### Structure

The guild bank operates through NPC intermediation, consistent with the [[Economy Model]] philosophy. All deposits and withdrawals are processed through the Guild Banker NPC in any [[Social Hubs|social hub]] (and via Guild Vault Access station in guild halls post-MVP).

| Feature | Detail |
|---|---|
| **Starting slots** | 50 (unlocked at guild level 5) |
| **Expanded slots** | 100 at level 20, 150 at level 30 |
| **Gold storage** | Unlimited gold pool (guild treasury) |
| **Deposit** | Any member can deposit gold or items |
| **Withdrawal** | Rank-gated with daily caps (see below) |

### Withdrawal Limits

| Rank | Gold per Day | Items per Day |
|---|---|---|
| Leader | Unlimited | Unlimited |
| Officer | 5,000 gold | 10 items |
| Veteran | 1,000 gold | 3 items |
| Member | 500 gold | 2 items |
| Recruit | 0 | 0 |

Custom ranks can be assigned withdrawal limits between Member and Officer tiers by the Leader. Leader can also set an **upkeep reserve** -- gold below this threshold cannot be withdrawn by anyone except the Leader.

### Transaction Log

- **Every deposit and withdrawal** is logged with: timestamp, player name, rank, amount/item, and remaining balance
- Log is visible to Leader and Officers by default; Leader can optionally make it visible to all members (transparency toggle)
- Log retains **90 days** of history
- Filterable by player, date range, transaction type (deposit/withdraw), and amount threshold

### Guild Bank Uses

| Use | Description |
|---|---|
| **Guild upkeep** | Automatic weekly deduction (see Guild Upkeep) |
| **Territory bids** | Gold spent to bid on contested territories at weekly auction |
| **Guild crafting orders** | Commission NPC crafters for guild-wide gear or consumables via [[Social Hubs#NPC Commission Board]] |
| **War declaration fee** | 500 gold per war declaration |
| **Guild hall upgrades** | Post-MVP, 2,000-25,000 gold per upgrade |

### Treasury Income Sources

| Source | Amount | Notes |
|---|---|---|
| Member deposits | Variable | Voluntary, tracked per member |
| Guild Wars victory | 500-2,000 gold | Scales with war format |
| Guild dungeon completion bonus | 5% of party gold reward | Only when 3+ guild members in party |
| Guild quest rewards | 200-500 gold | Weekly guild quests |

---

## Guild Upkeep

### Weekly Cost

Guild upkeep is deducted automatically from the guild treasury at weekly reset (same cycle as [[Economy Model]] weekly sinks).

| Factor | Cost |
|---|---|
| **Base upkeep (Band, 1-10 active)** | 1,000 gold/week |
| **Company tier (11-30 active)** | 2,500 gold/week |
| **Order tier (31-75 active)** | 4,000 gold/week |
| **Legion tier (76+ active)** | 5,000 gold/week |
| **Per owned territory** | +500 gold/week per territory |

**Note:** Only **active** members (logged in within 14 days) count toward the tier calculation. Inactive and dormant members do not increase upkeep costs.

**Examples:**

| Guild Profile | Weekly Upkeep |
|---|---|
| 8 active members, 0 territories | 1,000 gold |
| 25 active members, 0 territories | 2,500 gold |
| 50 active members, 1 territory | 4,500 gold |
| 90 active members, 3 territories | 6,500 gold |

This aligns with the [[Economy Model]] sink table (1,000-5,000 gold/week) for typical guilds. Only the largest territorial guilds exceed the baseline range.

### Upkeep Failure Consequences

Failure cascades gracefully. **Guilds are never forcibly dissolved.**

| Missed Weeks | Consequence |
|---|---|
| 1 | Warning notification sent to Leader and Officers |
| 2 | Lose control of territories (highest-cost territory first). Treasury is empty -- no partial payment. |
| 3 | Guild bank access reduced (cannot withdraw; deposits still allowed) |
| 4 | Guild perks suspended: banner reverts to default, leaderboard eligibility paused, guild quest access paused |
| 5+ | Guild enters **Dormant** status: tag hidden, cannot recruit, cannot declare wars. Roster and guild level preserved. |

**Recovery:** Depositing sufficient gold to cover the current week's upkeep immediately reactivates the guild. Features restore in reverse order of loss. Outstanding debt does NOT accumulate -- only the current week's upkeep must be covered.

---

## Recruitment

### Recruitment Board

The guild recruitment board is located in all [[Social Hubs]] (Port Haven, Sky Forum, Thunder Arena). Guild listings display:

- Guild name, tag, and faction affiliation
- Member count / member cap
- Guild level and tier name
- Guild description (up to 500 characters)
- Requirements (level, alignment, preferred domains, activity expectations)
- Recruitment status (Open / Application Required / Closed)
- Primary focus tags (PvP, PvE, Social, Crafting, Competitive)
- War record (wins/losses)

Higher guild level grants priority positioning on the recruitment board (guild level 10+ perk).

### Guild Finder

A searchable interface accessible from any hub panel.

| Filter | Options |
|---|---|
| Faction affiliation | Luminari, Hollow Court, Driftborn, Unaffiliated |
| Guild size | Band (1-20), Company (21-50), Order (51-100), Legion (101-150) |
| Focus | PvP, PvE, Social, Crafting, Competitive |
| Recruitment status | Open, Application Required |
| Guild level | Range slider (1-50) |
| Language | Dropdown |
| Activity level | High (70%+ active), Moderate (40-69%), Low (<40%) |
| Faction reputation | Minimum tier filter (e.g., "at least 5 members Honored+") |

### Application System

| Mode | Behavior |
|---|---|
| **Open** | Any eligible player can join instantly (up to member cap) |
| **Application Required** | Player submits a short application (up to 200 characters). Officers and Leader can accept/reject. Applications expire after 7 days if not acted upon. |
| **Closed** | No applications accepted. Invite-only. |

**Constraints:**

- Players can have **one pending application** at a time
- Players can be members of **one guild** at a time
- Leaving a guild imposes a **24-hour cooldown** before joining or applying to another
- Being kicked imposes a **48-hour cooldown** before applying to ANY guild

---

## Guild Wars

### Declaration

| Requirement | Detail |
|---|---|
| Guild level | 15+ |
| Declaring rank | Leader or Officer (if permitted) |
| Cost | 500 gold from each guild's treasury (returned to winner) |
| Acceptance | Defending guild has 24 hours to accept or decline (no penalty for declining) |
| Scheduling | Both guilds agree on a time window (prevents ambush griefing) |
| Concurrent wars | Maximum 2 active wars per guild |

### War Formats

| Format | Players | Duration | Guild Level Req |
|---|---|---|---|
| **Skirmish War** | 5v5 | Best of 5 matches, ~30 min | 15+ |
| **Siege War** | 10v10 | Objective-based, 45 min | 20+ |
| **Grand War** | 20v20 | Multi-objective campaign, 90 min | 30+ |

### War Mechanics

- Wars take place in instanced versions of [[World Design#Zone Contested Areas|contested zones]]
- Both guilds select their roster before the war begins (no mid-war substitutions)
- **Objectives vary by format:**
  - **Skirmish:** Elimination rounds. Best of 5.
  - **Siege:** Capture and hold 3 control points. Points scored for time held.
  - **Grand:** Multi-phase campaign -- capture points, escort payload, final base assault.
- Combat follows standard [[Combat System]] rules. No special war-only abilities.
- [[Combat Logs System|Combat logs]] are recorded for both guilds and available post-war for review.

### War Scoring (Grand War Detail)

| Objective | Points |
|---|---|
| Capture control point | 25 |
| Defend control point (repel attempt) | 15 |
| Payload delivery (per checkpoint) | 20 |
| Final base assault (successful) | 50 |
| Individual elimination | 1 |

Victory goes to the guild with the most points at the end of the time window. Ties go to the defender.

### Territory Control

Territories are specific zones on the world map that guilds can claim and contest. This is the primary material incentive for organized guild play.

| Feature | Detail |
|---|---|
| **Eligibility** | Guild level 20+ with territory bidding rights |
| **Bidding** | Weekly auction. Guilds place sealed bids from guild treasury. Highest bid wins. Bid gold consumed. |
| **Defense** | Owning guild must defend territory when challenged during war or weekly Alignment Wars |
| **Benefits** | +3% XP for guild members in controlled territory, +5% gold from activities in that zone, guild banner displayed in zone hub |
| **Limit** | Maximum 3 territories per guild |
| **Loss conditions** | Lost through war defeat (if territory was an objective), Alignment Wars, or upkeep failure |

**Territory buffs are minor and convenience-oriented.** Per [[Gameplay Loops]] design rules: "No direct power advantages. Snowballing guild dominance through power advantages is a design failure -- dominance should come from coordination and skill, rewarded with prestige and cosmetics." The 3-territory cap and escalating upkeep costs prevent snowballing.

### Faction Integration

- Guild wars between **same-faction** guilds are internal disputes -- no faction reputation impact
- Guild wars between **different-faction** guilds earn bonus faction reputation (200 rep per war participation, win or lose)
- Faction-aligned guilds fighting the opposing faction (Luminari vs Hollow Court) gain the existing +10% damage bonus in open-world PvP zones per [[Factions System#Cross-Faction Dynamics]]
- During weekly [[Factions System#Alignment Wars|Alignment Wars]], affiliated guilds contribute collectively to faction war points. Top contributing guild per faction earns a weekly "Vanguard" title displayed on guild tag.

### War Rewards

| Outcome | Reward |
|---|---|
| **Victory** | 500-2,000 gold to treasury (scales with format) + 500 Guild XP + War Victory cosmetic banner (displayed 1 week) |
| **Defeat** | 100 Guild XP (participation) |
| **Individual MVP** | Personal cosmetic title: "Warbreaker" (1 week duration) |

### Surrender

- Either guild can offer surrender at any point after the first match/phase
- Opposing guild must accept for surrender to take effect
- Surrendering guild forfeits any contested territory nodes captured during the war
- Entry fee gold goes to the winning (non-surrendering) guild
- No additional penalties beyond the entry fee loss

### War Seasons (Quarterly)

- Guilds accumulate war victories over a 3-month season
- Top 10 guilds per server earn seasonal rewards:
  - **1st place:** Exclusive guild emblem border + "Conqueror" guild title + 10,000 gold to treasury
  - **2nd-3rd:** Seasonal guild emblem accent + "Warlord" guild title
  - **4th-10th:** Seasonal cosmetic banner
- Season results are permanent guild history -- viewable in guild profile

---

## Guild Halls (Post-MVP)

Guild halls are **instanced guild spaces** planned for Phase 2 (Month 4-6) per [[Long-Term Vision]]. Accessible from any social hub portal.

### Core Features

| Feature | Description |
|---|---|
| **Instance** | Private guild zone, always available, state saved between sessions |
| **Capacity** | All guild members + up to 10 visitors |
| **Entrance** | Portal accessible from any [[Social Hubs|social hub]] |
| **Persistence** | Furniture, trophies, and customization persist across sessions |

### Customization

| Category | Options |
|---|---|
| **Layout** | 3 base layouts (Fortress, Lodge, Tower) expandable with guild level |
| **Visual theme** | Reflects faction affiliation (Radiant: gold/white, Corrupted: purple/crimson, Unbound: silver/blue, Unaffiliated: neutral stone) |
| **Furniture** | Decorative items purchased with gold or earned from achievements |
| **Banners** | Guild emblem and faction banners displayed throughout |
| **Entrance description** | Custom text visible to visitors (e.g., "You step into a hall of gleaming gold. War banners line the walls...") |

### Functional Stations

| Station | Function | Unlock |
|---|---|---|
| **Sparring Arena** | Private practice space, supports up to 5v5 | Guild level 20 (base hall) |
| **Guild Board** | Internal quest board with leader-posted objectives and bounties | Guild level 20 (base hall) |
| **Guild Bank Interface** | Direct guild bank access without traveling to a social hub NPC | Guild level 20 (base hall) |
| **Trophy Display** | Showcase guild achievements: war victories, raid completions, leaderboard placements, legendary gems | Guild level 25 (tier 2) |
| **Craft Workshop** | Shared crafting stations with +10% crafting speed bonus for guild members | Guild level 25 (tier 2), 10,000 gold |
| **Resonance Lab** | Guild members gain +5% resonance discovery chance while in guild hall | Guild level 25 (tier 2), 8,000 gold |
| **War Room** | Enhanced war planning: review past war logs, build comparison tools, strategy board | Guild level 25 (tier 2), 7,000 gold |
| **Faction Quartermaster** | Faction vendor with discounted faction recipes | Per [[Factions System]], requires 5+ members at Honored+ |
| **Display Pedestals** | Showcase legendary gems or gear (visible to visitors) | 3,000 gold each |

### Visitor Permissions

| Setting | Options |
|---|---|
| **Open** | Any player can visit (read-only, no station use) |
| **Friends Only** | Only friends of guild members can visit |
| **Closed** | Guild members only |
| **Station Access** | Visitors never have crafting, bank, or war room access |

---

## Guild Dissolution

### Leadership Transfer

| Scenario | Process |
|---|---|
| **Voluntary transfer** | Leader promotes an Officer to Leader. Instant. Former leader becomes Officer. |
| **Leader inactivity (30 days)** | Highest-ranking Officer with the longest tenure receives a transfer prompt. If no Officer acts within 7 days, the next Officer is prompted. |
| **No eligible successor** | If no Officer exists or all Officers are inactive (60+ days), any active Member can petition for leadership. Petition is granted after 7 days with no objection from other active members. |
| **All members inactive** | Guild enters permanent Dormant status. If the Leader returns, they can reactivate. After 180 days of total inactivity (all members), the guild name is released. Roster and progress are archived. |

### Guild Merging

There is no automated merge system. Recommended process:

1. One guild disbands, members join the other guild manually
2. Leader of the disbanding guild can transfer guild treasury gold to the receiving guild's bank via a one-time NPC merger transaction (**10% gold fee** as sink)
3. Guild XP, level, achievements, and war history **do not transfer** -- this prevents gaming the system by farming throwaway guilds

### Disbanding

| Step | Detail |
|---|---|
| **Initiation** | Leader only. Requires typing guild name as confirmation. |
| **Grace period** | 72-hour countdown. Leader can cancel during this period. All members are notified immediately. |
| **Gold distribution** | Remaining guild treasury gold is distributed equally among all active members (logged in within 14 days). Unclaimed shares held by Guild Banker NPC for 30 days, then destroyed. |
| **Item distribution** | Guild bank items can be withdrawn during the 72-hour grace period. Unclaimed items after disbanding are destroyed. |
| **Post-disband cooldown** | Leader cannot create a new guild for **7 days**. |
| **Name reservation** | Guild name is reserved for **90 days** after disbanding (prevents impersonation). |

---

## Cross-Guild Features

### Alliances

| Feature | Detail |
|---|---|
| **Alliance size** | 2-3 guilds per alliance |
| **Requirements** | Same faction affiliation (or all unaffiliated). Guild level 20+. |
| **Benefits** | Shared alliance chat channel, allied guild members visible on zone maps, +2% XP when grouped with allied guild members |
| **Restrictions** | Allied guilds cannot declare war on each other. Alliance does not share guild banks, territory, or guild XP. |
| **Formation** | All guild leaders must confirm. 48-hour formation period. |
| **Dissolution** | Any leader can withdraw with 48-hour notice. |
| **Limit** | A guild can be in only 1 alliance at a time. |

### Guild Leaderboards

Accessible from Sky Forum and guild panels. Updated weekly. Requires guild level 30+ to appear.

| Leaderboard | Metric |
|---|---|
| **PvP Dominance** | Total guild war victories + member PvP rating aggregate |
| **PvE Progression** | Highest mythic dungeon tier cleared + total boss kills |
| **Territory Control** | Number of territories held + consecutive weeks held |
| **Activity** | Total guild XP earned in current week |
| **Wealth** | Total gold flowing through guild bank (deposits, not balance -- prevents hoarding incentive) |

Top 3 guilds per leaderboard receive a cosmetic badge on their guild banner for the following week.

### Inter-Guild Tournaments

| Feature | Detail |
|---|---|
| **Format** | Bracketed elimination. 3v3 Skirmish format. Each guild fields one team. |
| **Entry** | Guild level 15+. 1,000 gold entry fee from guild treasury. |
| **Schedule** | Monthly. Registration opens 1 week before. |
| **Bracket size** | 16 guilds per tournament. If more register, qualifying round by guild PvP leaderboard rank. |
| **Rewards** | Winner: 10,000 gold to treasury, exclusive "Tournament Champion" banner, +1,000 guild XP. Runner-up: 5,000 gold, +500 guild XP. All participants: +200 guild XP. |
| **Spectating** | All matches viewable via spectator mode (live combat log, per [[PvP Structure]]) |

---

## Content Integration

### Dungeon Bonuses

When 3+ guild members form a dungeon party:

- +5% gold bonus (deposited directly to guild treasury)
- Guild XP earned per clear
- Shared [[Combat Logs System|combat log]] review available in guild log post-dungeon
- **No combat stat bonuses** -- coordination advantage is earned through communication, not numbers

### PvP Integration

- 3v3 Skirmish can be queued as a guild team -- earns guild reputation alongside personal honor
- Arena League faction reputation (see [[Factions System#Stormreach Arena League]]) counts toward guild faction milestones
- Guild War results feed into the quarterly War Season leaderboard

---

## Balance Guardrails

| Rule | Rationale |
|---|---|
| No guild combat stat bonuses | Prevents guilded vs unguilded power gap |
| Faction Rally (+3% stats) only in faction-aligned zones | Geographic advantage, not universal power |
| Guild dungeon bonus is gold only, not drop rate | Economic convenience, not progression advantage |
| War rewards are cosmetic + gold, not exclusive gear or gems | Wars are for prestige, not gating content |
| Guild XP daily cap per member (2,000) | Prevents mega-guild size from being the dominant leveling strategy |
| Territory buffs capped at +3% XP and +5% gold | Minor convenience, prevents snowballing |
| Maximum 3 territories per guild | Hard cap on territorial dominance |
| Treasury withdrawal caps and transaction logs | Prevents officer theft, builds trust |
| War scheduling required | Prevents grief-declaration and timezone abuse |
| Dormant status preserves roster and level | Guilds that take breaks do not lose everything |
| No forced dissolution | Upkeep failure degrades features, never destroys the guild |

---

## MVP Scope

Guild system launches in two phases per [[MVP Design]]:

### MVP (Launch)

- Guild creation, naming, tags, description
- Default rank system (Leader, Officer, Veteran, Member, Recruit) with fixed permissions
- Guild bank (50 slots, basic deposit/withdraw)
- Guild roster with activity tracking
- Guild treasury (deposits, upkeep, basic expenses)
- Recruitment board listing in Port Haven (open/closed toggle only)
- Guild log (joins, leaves, promotions, treasury activity)
- Guild chat
- Dungeon party bonus (+5% gold with 3+ guild members)
- Guild progression to level 20

### Post-MVP (Phase 2 Expansion)

- Custom ranks and full permission matrix
- Guild Wars (all three formats) and War Seasons
- Territory bidding and control
- Guild halls with customization and functional stations
- Faction affiliation and all faction-guild features
- Guild quests (weekly objectives)
- Guild progression to level 50
- Alliance system
- Inter-guild tournaments
- Full guild finder with filters and application system
- Expanded recruitment board in all social hubs
- Guild emblems and cosmetic upgrades

---

## Related Pages

- [[Factions System]] -- Faction affiliation, faction rally, quartermaster, champion slot
- [[Social Hubs]] -- Recruitment board locations, guild hall directory
- [[Economy Model]] -- Guild upkeep as gold sink
- [[Gameplay Loops]] -- Guild Wars and guild progression in meta loop
- [[PvP Structure]] -- Skirmish guild teams, guild reputation, spectator mode
- [[Long-Term Vision]] -- Guild halls Phase 2 timeline
- [[MVP Design]] -- Guild feature phasing
- [[Alignment System]] -- Guild alignment contribution
- [[Combat Logs System]] -- War log recording and post-dungeon review
- [[World Design]] -- Contested zones used for Guild Wars
- [[Creator Ecosystem]] -- Guild build sharing and coordination
