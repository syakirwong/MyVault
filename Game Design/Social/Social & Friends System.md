---
tags:
  - gdd
  - social
  - multiplayer
---

# Social & Friends System

Player-to-player relationship infrastructure: friend lists, parties, LFG, inspect, and block systems. This document covers the social graph layer -- how players find, connect with, and group with each other.

---

## Design Principles

1. **Mutual Consent** -- all social connections require both parties to agree. No one-way follows, no unsolicited visibility.
2. **Low Friction Grouping** -- forming a party or finding a group should take seconds, not minutes.
3. **Privacy by Default** -- players control who sees their information. Sensible defaults protect new players.
4. **No Direct Transfer** -- social features never bypass the [[Trade System]]'s NPC-mediated economy. Friendship does not create a trade channel.

---

## Friend List

### Adding Friends

| Method | Flow |
|---|---|
| By name | Open Social Panel > Add Friend > Type exact character name > Send request |
| From player list | Click player name in hub player list > Send Friend Request |
| From recent players | Open Recent Players tab > Send Friend Request |
| Post-match | After PvP or dungeon completion > party member names shown > Send Friend Request |

### Friend Request Flow

```
Player A sends request → Player B receives notification (Social category)
                        → Player B can: Accept / Decline / Ignore

Accept  → Both players appear on each other's friend list (mutual)
Decline → Request removed. Player A is NOT notified of decline (reduces social pressure)
Ignore  → Request sits in pending queue. Auto-expires after 7 days

Player A can cancel a pending request at any time
```

### Friend Model

- **Mutual only.** Both players must accept for the connection to exist. There is no one-way follow or asymmetric relationship.
- **Friend cap:** 200 per player.
- **Remove friend:** Either player can remove the other at any time. The removed player is NOT notified. They simply disappear from each other's lists.

### Friend List Display

| Column | Content |
|---|---|
| **Name** | Character name with alignment badge color (see [[UI Design]]) |
| **Status** | Online status indicator (see below) |
| **Location** | Current region name (if status allows) |
| **Level** | Character level |
| **Actions** | Whisper, Invite to Party, Inspect, Remove |

Friends are sorted: Online first (alphabetical), then Offline (alphabetical). AFK and In Combat/In Dungeon count as online sub-states.

---

## Online Status

Players broadcast a status visible to their friends list.

| Status | Icon | Description | Shows Location? |
|---|---|---|---|
| **Online** | Green | In-game, available | Yes |
| **In Combat** | Red | Currently in a combat encounter | Yes (zone only, not dungeon name) |
| **In Dungeon** | Orange | Inside a dungeon instance | Yes (dungeon name) |
| **AFK** | Yellow | Idle for 5+ minutes (auto-set) or manually set | Yes |
| **Appear Offline** | Gray | Player appears as offline to all friends | No |

### Status Rules

- Status is **automatic** by default. The game sets In Combat and In Dungeon based on player state. AFK triggers after 5 minutes of no input.
- Players can **manually override** to AFK or Appear Offline at any time.
- Appear Offline is complete: the player does not appear in hub player lists, friend lists show them as offline, and whispers are held until they return to a visible status.
- Appear Offline players can still send whispers and join parties (their status changes to Online when they accept a party invite).

---

## Party System

### Formation

| Method | Flow |
|---|---|
| Invite by name | Party Panel > Invite > Type character name |
| Invite from friend list | Friend List > click Invite to Party |
| Invite nearby | Hub player list > click player name > Invite to Party |
| LFG auto-match | LFG system forms the party automatically (see LFG section) |

### Party Size

| Context | Max Size | Reason |
|---|---|---|
| Dungeon content | 4 | Balanced for [[PvE Structure]] encounter design |
| Open world | 8 | Flexible grouping for world bosses, faction events |
| Arena PvP (3v3) | 3 | Per [[PvP Structure]] |
| Arena PvP (1v1) | 1 | Solo queue only |

### Party Roles

| Role | Permissions |
|---|---|
| **Leader** | Invite, kick, set loot rules, queue for content, disband, promote leader |
| **Member** | Leave, suggest invite (leader must confirm), view party info |

- The player who creates the party is the leader.
- Leader can **promote** another member to leader. Only one leader at a time.
- If the leader disconnects, leadership auto-transfers to the longest-tenured member after 2 minutes.

### Party Actions

| Action | Who Can | Effect |
|---|---|---|
| **Invite** | Leader | Sends party invite notification to target player |
| **Kick** | Leader | Removes player from party. Kicked player cannot rejoin for 5 minutes |
| **Leave** | Any member | Player leaves voluntarily. No penalty |
| **Disband** | Leader | Dissolves the entire party. All members notified |
| **Promote** | Leader | Transfers leadership to another member |

### Party Loot Rules

> [!info] Full Specification
> See [[Loot System]] for complete loot distribution rules, drop tables, and party loot options.

The party leader selects a loot rule before entering instanced content:

| Rule | Behavior |
|---|---|
| **Round Robin** | Items distributed in rotation. Default |
| **Need/Greed** | Members roll Need (can use) or Greed (want for value). Need wins over Greed. Ties broken randomly |
| **Free for All** | First to loot claims the item |

Loot rules are locked once the party enters a dungeon instance and cannot be changed mid-run.

### Party Chat

- Party chat is a dedicated channel visible only to party members.
- Party chat persists as long as the party exists.
- Party chat history is cleared when the party disbands.
- Party members in different zones can still use party chat.

---

## Recent Players

A system-maintained list of players the user has recently interacted with.

### Sources

| Interaction | Added to Recent? | Duration Kept |
|---|---|---|
| Party member (dungeon or open world) | Yes | 7 days |
| PvP opponent (arena) | Yes | 7 days |
| Hub interaction (inspected or whispered) | Yes | 3 days |
| Dueling opponent | Yes | 3 days |
| Traded via NPC commission (crafter/buyer) | No (anonymous by design) | -- |

### Recent Players List

- Shows up to **50** most recent players.
- Oldest entries are evicted when the cap is reached.
- Each entry shows: character name, level, alignment badge, context ("Dungeon: Tidecaller Caverns", "Arena: 3v3"), and timestamp.
- **Quick actions:** Send Friend Request, Whisper, Block.
- Recent players list is local to the client and not persisted across devices (privacy consideration).

---

## Player Inspect

View another player's build card and public profile information.

### What Inspect Shows

| Section | Contents | Source |
|---|---|---|
| **Build Card** | Equipped gem domains, alignment, alignment depth tier, archetype name | [[Behavioral Identity]], [[UI Design]] |
| **Stats Summary** | Level, total mastery, faction standings (tier names only, not exact points) | [[Progression Systems]] |
| **Creator Profile** | Published builds count, creator level (if applicable) | [[Creator Ecosystem]] |
| **Cosmetics** | Active title, name plate style, combat log skin | [[Cosmetic System]] |
| **Guild** | Guild name and rank (if in a guild) | [[Guild System]] |

### Inspect Does NOT Show

- Exact gem details (specific modifiers, mutations, mastery levels) -- this is competitive information
- Gold or currency amounts
- Inventory contents
- Friend list
- Exact faction reputation points

### Where Inspect Is Available

| Location | Available? | Notes |
|---|---|---|
| Social hubs | Yes | Click any player in the player list |
| Open world zones | Yes | Target nearby players |
| Post-PvP results | Yes | Inspect opponents from the match results screen |
| During combat | No | Cannot inspect mid-fight |
| Dungeon (out of combat) | Yes | Inspect party members |

### Privacy Settings

Players control who can inspect them.

| Setting | Who Can Inspect | Default? |
|---|---|---|
| **Everyone** | Any player | Yes (default) |
| **Friends Only** | Only players on your friend list | No |
| **Nobody** | No one can inspect you | No |

Privacy settings are changed in Settings > Social > Inspect Privacy.

Regardless of privacy setting, the player's **name, level, and alignment badge** are always visible in hub player lists (this is required for the hub to function per [[Social Hubs]]).

---

## Block System

Blocking severs all social interaction channels with a specific player.

### What Blocking Does

| Channel | Effect |
|---|---|
| **Whispers** | Blocked player cannot send you whispers. Your whispers to them are also suppressed |
| **Party Invites** | Blocked player cannot invite you. You cannot invite them |
| **Chat** | Blocked player's messages are hidden in all public chat channels (region, hub) |
| **Inspect** | Blocked player cannot inspect you. You cannot inspect them |
| **Friend Requests** | Blocked player cannot send you friend requests |
| **LFG Matching** | You will not be matched with blocked players in LFG auto-match |
| **Hub Player List** | Blocked player does not appear in your player list. You do not appear in theirs |

### What Blocking Does NOT Do

- Block does not prevent the blocked player from seeing you in PvP matchmaking (competitive integrity). You can be matched as opponents.
- Block does not prevent being in the same hub instance (server infrastructure limitation). It only hides visibility.
- Block does not notify the blocked player that they have been blocked.

### Block List Management

- **Block cap:** 500 players.
- Access: Settings > Social > Block List.
- Each entry shows: character name, date blocked, context note (optional, private).
- **Unblock:** Removes all restrictions. Does not automatically re-add as friend.
- Blocking a current friend removes them from your friend list first, then applies the block.

---

## LFG (Looking for Group)

A matchmaking and browsing system for finding groups for specific content.

### LFG Board

The LFG board is accessible from any hub via the Social Panel.

#### Posting a Listing

Players create an LFG listing with the following fields:

| Field | Options | Required? |
|---|---|---|
| **Content Type** | Dungeon (select name), PvP (1v1 / 3v3), World Boss, Open World Group | Yes |
| **Difficulty** | Normal, Hard, Mythic (for dungeons) | Yes (if dungeon) |
| **Playstyle Tags** | Up to 3 tags from: Damage, Sustain, Control, Support, Hybrid, Speed Run, Learning | Yes (at least 1) |
| **Note** | Free-text, 100 character max | No |
| **Party Status** | Solo (looking for full group), Have Party (need X more) | Yes |

> [!note] No Class Roles
> Stormbound Online has no classes. LFG uses **playstyle tags** instead of role queues. A player tagging "Sustain + Control" signals their build's function without requiring a tank/healer/DPS framework. This aligns with the game's [[Gem Identity System]] philosophy where builds emerge from gem combinations, not predetermined roles.

#### Browsing Listings

- Filter by: Content Type, Difficulty, Playstyle Tags.
- Sort by: Newest, Party Size (most filled first).
- Each listing shows: poster name, party size (current/max), playstyle tags, note, time posted.
- **Apply:** Click to request joining. Poster receives notification and can Accept or Decline.

### Auto-Match Queue

For players who prefer speed over selection.

| Step | Action |
|---|---|
| 1 | Select content type and difficulty |
| 2 | Select your playstyle tags (1-3) |
| 3 | Enter queue |
| 4 | System matches players aiming for tag diversity (avoids 4x Damage parties when possible) |
| 5 | Match found > notification (urgent priority, see [[Notification & Mail System]]) |
| 6 | All matched players have 60 seconds to accept. If any player declines or times out, remaining players re-enter queue with priority |

### Auto-Match Composition Logic

The system attempts to compose parties with **tag diversity** but does not enforce strict composition. Priority order:

1. At least 1 player tagged "Sustain" or "Support" (soft preference, not hard requirement)
2. No more than 3 players sharing the exact same tag set (reduces redundancy)
3. Minimize queue time -- after 3 minutes, composition preferences relax. After 5 minutes, any valid group is formed

### LFG Rules

| Rule | Detail |
|---|---|
| Max active listings | 1 per player |
| Listing expiry | 30 minutes (renewable) |
| Queue timeout | 10 minutes (player notified, can re-queue) |
| Cross-hub | LFG is server-wide, not hub-specific |
| Blocked players | Never matched together in auto-match |

---

## Social Panel Layout

The Social Panel is accessible from the Hub Panel and contains all social features in tabbed navigation.

| Tab | Contents |
|---|---|
| **Friends** | Friend list with status, search, add friend button |
| **Party** | Current party members, invite button, loot settings, leave/disband |
| **Recent** | Recent players list with quick actions |
| **LFG** | LFG board (browse/post) and auto-match queue |
| **Blocked** | Block list management |

---

## MVP Scope

For MVP (see [[MVP Design]]), the following social features are included:

| Feature | MVP? | Notes |
|---|---|---|
| Friend list + requests | Yes | Core social infrastructure |
| Online status | Yes | 5 statuses as specified |
| Party system (4-player) | Yes | Required for dungeon content |
| Party chat | Yes | Required for coordination |
| Player inspect | Yes | Already referenced in [[Social Hubs]] |
| Recent players | Yes | Low effort, high social value |
| Block system | Yes | Required for player safety |
| LFG board (manual) | Yes | Manual browse and post |
| LFG auto-match | Post-MVP | Requires matchmaking infrastructure |
| 8-player open world party | Post-MVP | No world boss content at MVP |

---

## Related Pages

- [[Social Hubs]]
- [[Guild System]]
- [[Creator Ecosystem]]
- [[UI Design]]
- [[Trade System]]
- [[PvP Structure]]
- [[PvE Structure]]
- [[Notification & Mail System]]
- [[Loot System]]
- [[Behavioral Identity]]
