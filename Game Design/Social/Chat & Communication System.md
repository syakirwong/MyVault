---
tags:
  - gdd
  - social
  - communication
---

# Chat & Communication System

The unified specification for all player-to-player text communication in Stormbound Online. Chat is a social system, not a power system — it connects players, surfaces builds, and coordinates group play without granting mechanical advantage.

---

## Design Principles

1. **Clarity Over Clutter** — channels are distinct, purposes are obvious, noise is filterable
2. **Build Sharing is Native** — build codes and item links are first-class message objects, not raw text
3. **Safety by Default** — new players are protected from global spam; moderation is layered and escalating
4. **No Pay-to-Chat** — all chat features are earned through gameplay, never purchased

---

## Chat Channels

### Channel Overview

| Channel | Scope | Access Requirement | Color | Default |
|---------|-------|-------------------|-------|---------|
| **Region** | All players in current region | None | White | Yes (default channel) |
| **Party** | Party members only (up to 5) | Join a party | Cyan | No |
| **Guild** | Guild members only | Join a guild | Green | No |
| **Faction** | Faction-aligned players (server-wide) | Exalted faction reputation | Faction color (see [[Factions System]]) | No |
| **Whisper** | 1-to-1 private | None | Light Purple | No |
| **Trade** | All players in current region | Character level 10+ | Gold | No |
| **Global** | All players on server | Character level 10+ | Silver | No |

### Channel Details

#### Region Chat

- Visible to all players in the same region (e.g., Calm Shores, Stormreach)
- No level requirement — first channel available to new players
- Scoped to region boundaries; changing regions changes the region chat feed
- Hub capacity limits (see [[Social Hubs]]) do not affect region chat — chat covers the full region, not just the hub instance
- Default channel on login and region entry

#### Party Chat

- Available when in a party (2-5 players)
- Messages visible only to current party members
- Automatically selected when joining a party; reverts to Region when party disbands
- Persists across regions — party members in different zones can still communicate
- Party leader can toggle party chat to "quiet mode" (suppresses non-party channels in the chat panel)

#### Guild Chat

- Available to all guild members regardless of rank (see [[Guild System#Ranks and Permissions]])
- Recruits (7-day probation) have guild chat access but cannot post in war council chat
- Guild Message of the Day displays on first guild chat open per session
- Officers+ can pin messages (max 3 pinned at once)
- **War Council Chat** — sub-channel within guild chat, available at Guild Level 15+. Restricted to Officer+ rank by default (configurable by Founder)

#### Faction Chat

- Server-wide channel restricted to Exalted-reputation members of the corresponding faction
- Three separate faction channels exist, one per major faction:
  - **Luminari War Council** — Luminari Exalted members
  - **Court of Whispers** — Hollow Court Exalted members
  - **The Third Path Council** — Driftborn Exalted members
- Cross-faction chat does not exist by design — factions are rivalries, not alliances
- If a player switches alignment and loses Exalted status (50% rep reset per [[Factions System#Reputation Decay]]), faction chat access is revoked until re-earned

#### Whisper (Private Messages)

- 1-to-1 direct communication between any two players
- No level requirement
- Recipient must be online (no offline messaging in MVP)
- Privacy controls determine who can whisper you (see [[#Privacy Settings]])
- Whisper conversations are tracked per-recipient as separate tabs

#### Trade Chat

- Region-scoped channel dedicated to buying, selling, and trading
- Requires character level 10+ to post (readable at any level)
- Supports item linking and build code sharing (see [[#Structured Message Objects]])
- Enforced cooldown: 1 message per 30 seconds (prevents listing spam)
- No cross-region trade chat — players must be in the same region

#### Global Chat

- Server-wide general discussion
- Requires character level 10+ to post (readable at any level)
- Higher rate limiting than other channels (see [[#Rate Limiting]])
- Not region-scoped — all messages visible to all eligible players on the server

### Channel Switching

- **Keyboard shortcut:** `/r` (Region), `/p` (Party), `/g` (Guild), `/f` (Faction), `/w [player]` (Whisper), `/t` (Trade), `/gl` (Global)
- **Tab click:** Each active channel appears as a tab in the chat panel
- **Sticky selection:** Last-used channel persists as the active input channel until manually switched
- **Auto-switch:** Joining a party auto-switches to Party chat. Receiving a whisper opens a whisper tab but does not switch input focus

### Channel Visibility Rules

| Scenario | Visible Channels |
|----------|-----------------|
| New player (level 1-9) | Region, Party, Whisper |
| Player level 10+ | Region, Party, Whisper, Trade, Global |
| Guild member | + Guild (and War Council if rank permits) |
| Exalted faction member | + Faction |
| Solo, no guild, level 1 | Region, Whisper only |

---

## Message Format

### Character Limits

| Channel | Max Characters | Rationale |
|---------|---------------|-----------|
| Region | 300 | General conversation, moderate length |
| Party | 500 | Tactical coordination needs more space |
| Guild | 500 | Coordination and announcements |
| Faction | 300 | Strategic discussion, focused |
| Whisper | 500 | Private conversation, fewer constraints |
| Trade | 200 | Listings should be concise |
| Global | 300 | High-traffic channel, brevity enforced |

### Text Formatting

Players can use limited formatting in chat messages:

| Format | Syntax | Rendered As | Available In |
|--------|--------|-------------|-------------|
| Bold | `**text**` | **text** | All channels |
| Italic | `*text*` | *text* | All channels |
| Build code | `` `CODE` `` | Clickable build link | All channels |
| Item link | `[Item Name]` | Colored item reference (rarity color) | All channels |

No underline, strikethrough, headers, or custom colors. Chat is not a rich text editor.

### Link Policy

- **External links are blocked.** URLs beginning with `http://`, `https://`, or `www.` are stripped from messages and replaced with `[link removed]`
- **Rationale:** Phishing prevention in a game with tradeable items and currency
- **Alternative:** Community resources are shared via in-game build codes, item links, and the [[Creator Ecosystem]] which provides in-game build browsing

### Structured Message Objects

#### Build Code Sharing

- Players can paste a build code (e.g., `DUE-STM-CRT-PHZ-COR`) in backtick syntax
- The code renders as a clickable inline object showing: archetype name, domain icons, alignment badge
- Clicking opens the Build Panel inspection view for that loadout
- One build code per message maximum

#### Item Linking

- Players can link items from their inventory using `[Item Name]` syntax (auto-completed from inventory)
- Linked items display in their rarity color (see [[UI Design#Rarity Indicators]])
- Hovering or clicking shows item tooltip: stats, rarity, gem slots, crafter name (if applicable)
- Up to 3 item links per message

---

## Chat UI

### Panel Layout

The chat panel is part of the Hub Panel (see [[UI Design#Hub Panel]]) and persists across all non-combat contexts.

| Property | Default | Configurable |
|----------|---------|-------------|
| **Position** | Bottom-left of screen | Yes — bottom-left or bottom-right |
| **Width** | 35% of screen width | Yes — 25% to 50% |
| **Height** | 30% of screen height | Yes — 20% to 50% |
| **Opacity** | 90% | Yes — 50% to 100% |
| **Font size** | Medium | Yes — Small, Medium, Large |

### Tab System

- Each active channel appears as a tab at the top of the chat panel
- Tabs can be reordered by dragging
- Custom tabs: players can create a merged tab combining multiple channels (e.g., "Social" tab showing Region + Guild)
- Maximum 8 tabs visible simultaneously; overflow accessed via dropdown

### Timestamps

| Setting | Display |
|---------|---------|
| Off (default) | No timestamps shown |
| Relative | "2m ago", "1h ago" |
| Absolute | `[14:32]` in local time |

Toggled in chat settings. Timestamps apply to all channels uniformly.

### Scrollback

- **Depth:** 200 messages per channel retained in the client
- **Scroll behavior:** New messages auto-scroll unless the player has scrolled up (reading history)
- **"Jump to latest" button** appears when scrolled up and new messages arrive
- **Search:** No in-client chat search at MVP. Post-MVP consideration

### Unread Indicators

| Indicator | Behavior |
|-----------|----------|
| **Tab badge** | Numeric count of unread messages on inactive tabs (caps at 99+) |
| **Whisper flash** | Whisper tab pulses with light purple highlight until opened |
| **Mention highlight** | Messages containing `@YourName` highlighted with a distinct background color (pale gold) |
| **Tab dot** | Colored dot on tab for channels with any unread content (less prominent than badge) |

### Mention System

- `@PlayerName` highlights the message for the mentioned player
- Auto-complete suggestions appear after typing `@` + 3 characters
- Mentions trigger a subtle audio cue (toggleable in settings)
- `@party` mentions all party members; `@guild` mentions all online guild members (Officer+ only for `@guild`)
- No `@everyone` or `@global` — mass mentions are restricted to prevent abuse

---

## Social Features

### Friend List Integration

- Friends display an online status indicator next to their name in chat
- Whisper auto-complete prioritizes friends list
- Friends' messages in Region/Global chat are subtly highlighted (faint border, configurable)
- Right-click a player name in chat to: Add Friend, Whisper, Inspect Build, Invite to Party, Block

### Online Status

| Status | Display | Behavior |
|--------|---------|----------|
| **Online** | Green dot | Visible in friends list, searchable, can receive whispers |
| **In Combat** | Orange dot | Visible, but whispers are queued until combat ends |
| **AFK** | Yellow dot | Auto-set after 10 minutes of inactivity. Manual toggle available |
| **Appear Offline** | Gray dot (to self only) | Invisible to all players. Cannot receive whispers. Still in guild/party chat if applicable |

### Party Invite via Chat

- Right-click any player name in any chat channel to send a party invite
- Invite appears as a system message in the recipient's chat: `[PlayerName] has invited you to a party. [Accept] [Decline]`
- Clickable accept/decline buttons inline with the message

### Guild Recruitment Messages

- Guild Officers+ can post formatted recruitment messages in Region or Global chat
- Recruitment messages use a standardized template: `[GUILD] [Tag] GuildName — Description | Requirements | [Apply]`
- The `[Apply]` button opens the guild's recruitment board entry directly
- Recruitment messages are rate-limited: 1 per guild per channel per 10 minutes
- Recruitment messages are visually distinct (guild emblem color border) to differentiate from regular chat

### LFG (Looking for Group) System

- Players can post LFG requests via `/lfg` command
- LFG messages appear in Region chat with a structured format:

```
[LFG] PlayerName | Stormspire (Expedition) | 2/4 | "Need healer + DPS" | [Join]
```

- LFG posts persist for 15 minutes or until the party fills, whichever comes first
- Clicking `[Join]` sends a party request to the LFG poster
- `/lfg list` shows all active LFG posts in the current region
- LFG posts are separate from regular chat flow — they appear in a collapsible LFG section at the top of the Region chat tab

---

## Moderation Tools

### Profanity Filter

| Setting | Behavior |
|---------|----------|
| **Strict** | Replaces profanity and slurs with `***`. Includes common evasion patterns (leetspeak, spacing) |
| **Standard** (default) | Replaces severe slurs and hate speech. Mild profanity allowed |
| **Off** | No filtering. Player sees all text as written. Only available for players 18+ (age-gated in account settings) |

The filter is **client-side per recipient** — each player controls their own filter level. The sender's message is stored unfiltered; the recipient's client applies the filter on display.

### Spam Detection and Throttling

| Rule | Threshold | Action |
|------|-----------|--------|
| Duplicate message | Same message sent 3 times within 60 seconds | Block subsequent duplicates, warn sender |
| Rapid fire | More than 5 messages in 10 seconds | Throttle to 1 message per 5 seconds for 60 seconds |
| Global chat flood | More than 10 messages in 60 seconds (Global channel) | 5-minute cooldown on Global chat |
| Trade spam | More than 2 Trade messages in 60 seconds | 2-minute cooldown on Trade chat |
| Character spam | Message is 80%+ repeated characters | Block message, warn sender |

### Automated Toxicity Detection

- Server-side analysis of message content using pattern matching and heuristic scoring
- **Tier 1 (Low confidence):** Flagged for review but delivered. No player-facing action
- **Tier 2 (Medium confidence):** Message delivered with a private warning to sender: "This message may violate community guidelines"
- **Tier 3 (High confidence):** Message blocked. Sender receives: "Message not sent — content violation detected. If you believe this is an error, contact support"
- False positive rate target: below 2% for Tier 3 blocks
- All Tier 2 and Tier 3 actions logged for human review

### Player-Facing Block and Mute

| Action | Effect | Duration | Scope |
|--------|--------|----------|-------|
| **Mute** | Target player's messages hidden in all channels | Until manually unmuted | Per-player, client-side |
| **Block** | Mute + cannot send whispers + cannot send party invites | Until manually unblocked | Per-player, server-enforced |

- Muted/blocked players are not notified
- Block list capacity: 200 players
- Blocked players cannot inspect your build (privacy protection)

### Report Flow

Right-click a message or player name and select "Report" to open the report dialog.

**Report Categories:**

| Category | Description | Priority |
|----------|-------------|----------|
| **Harassment** | Targeted abuse, threats, bullying | High |
| **Hate Speech** | Slurs, discrimination, extremism | Critical |
| **Spam** | Advertising, repetitive messages, bot behavior | Medium |
| **Cheating** | Exploits, hacks, unfair advantage claims | High |
| **Inappropriate Content** | Sexual content, graphic descriptions, disturbing material | High |
| **Impersonation** | Pretending to be staff, another player, or NPC | Medium |
| **Other** | Freeform description | Low |

**Report Flow:**

1. Player selects category and optionally adds context (200 characters)
2. System captures the reported message + 10 messages of surrounding context (automatic)
3. Report enters the moderation queue with priority based on category
4. Reporter receives a system message within 24 hours: "Your report has been reviewed. Action has been taken" (no specifics disclosed to protect privacy)
5. Repeat reporters with 90%+ valid report rate gain "Trusted Reporter" status — their reports are prioritized

---

## Anti-Abuse

### Rate Limiting (Full Specification)

| Channel | Messages per Minute | Burst Allowance | Cooldown on Exceed |
|---------|--------------------|-----------------|--------------------|
| Region | 10 | 3 rapid messages | 30-second cooldown |
| Party | 20 | 5 rapid messages | 15-second cooldown |
| Guild | 15 | 4 rapid messages | 20-second cooldown |
| Faction | 8 | 2 rapid messages | 45-second cooldown |
| Whisper | 15 per recipient | 3 rapid messages | 20-second cooldown |
| Trade | 2 | No burst | 30-second cooldown |
| Global | 6 | 2 rapid messages | 60-second cooldown |

### New Account Restrictions

| Milestone | Chat Access Granted |
|-----------|-------------------|
| Account creation | Region chat (read + write), Whisper (friends only), Party chat |
| Character level 5 | Whisper (anyone, subject to privacy settings) |
| Character level 10 | Trade chat, Global chat |
| Join a guild | Guild chat |
| Exalted faction rep | Faction chat |

**Rationale:** Bots and gold sellers are economically incentivized to spam Trade and Global. The level 10 gate requires approximately 2-3 hours of gameplay, making throwaway bot accounts costly. Region chat remains open so new players are never isolated.

### Shadow-Muting

For repeat offenders who have received multiple valid reports but whose behavior does not warrant a ban:

- **Trigger:** 3+ validated reports within 7 days
- **Effect:** The player's messages in Region, Trade, and Global are visible only to themselves. Party, Guild, and Whisper are unaffected
- **Duration:** 24 hours (first offense), 72 hours (second), 7 days (third), permanent pending review (fourth)
- **Notification:** None. The player is not informed they are shadow-muted
- **Review:** All shadow-mutes are logged. Human moderator reviews within 48 hours of application. Can be reversed if deemed unjust

### Moderation Escalation

| Level | Trigger | Action | Handled By |
|-------|---------|--------|------------|
| **Automated** | Pattern match, spam detection, toxicity heuristic | Warning, throttle, message block | System |
| **Community** | Player reports | Report queue entry, priority sorting | System + Moderator |
| **Moderator** | 3+ reports on same player in 24h, or any Critical-priority report | Manual review, temporary mute (1-24h), warning | Human moderator |
| **Senior Moderator** | Repeat offender (3+ moderation actions in 30 days), ban consideration | Extended mute, temporary ban (1-7 days), permanent ban | Senior moderator |
| **Appeal** | Player contests a moderation action | Full review of logs, context, and history | Appeal team (separate from original moderator) |

### Appeal Process

- Players can appeal any moderation action via the in-game support menu
- Appeal requires: player name, date of action, description of dispute (500 characters)
- Appeal reviewed by a moderator who was not involved in the original action
- Response within 72 hours
- Outcomes: action upheld, action reversed, action modified (e.g., ban reduced to mute)
- Each player gets 1 appeal per moderation action. No re-appeals on the same incident

---

## Privacy

### Chat History Retention

| Type | Retention | Notes |
|------|-----------|-------|
| **Region, Trade, Global** | 30 days server-side | Purged on rolling basis |
| **Party** | Session only | Cleared when party disbands |
| **Guild** | 90 days server-side | Longer retention for guild coordination history |
| **Faction** | 30 days server-side | Same as public channels |
| **Whisper** | 30 days server-side | Required for harassment report investigation |
| **Reported messages** | 1 year | Retained for moderation and appeal purposes |

Players cannot export or download their chat history. Chat is ephemeral by design.

### Whisper Privacy Settings

Configurable in account settings under Privacy:

| Setting | Behavior |
|---------|----------|
| **Anyone** | All players can whisper you (subject to block list) |
| **Friends Only** (default) | Only players on your friends list can whisper you |
| **Guild & Friends** | Friends list + guild members can whisper you |
| **Nobody** | Whispers disabled entirely. You cannot send or receive |

Party members can always whisper each other regardless of setting (for tactical coordination). This override is documented in the setting tooltip.

### Appear Offline

- Toggleable in the status menu
- When active: player does not appear in friend lists, region player lists, or guild online rosters
- Whispers are blocked (sender receives: "Player is offline")
- Guild chat and party chat still function if the player is in a guild/party — appear offline hides presence, not participation
- Guild Officers can see a "[Name] (hidden)" indicator in the guild roster to prevent confusion about member counts

### Opt-Out of Region Chat

- Players can hide Region chat entirely from their chat panel
- Region chat tab is removed; no messages from the channel are displayed
- Does not affect other channels
- Re-enable at any time in chat settings
- System does not announce opt-out to other players

---

## MVP Scope

### MVP (Launch)

- Region chat, Party chat, Guild chat, Whisper
- Basic profanity filter (Standard mode only)
- Spam detection and rate limiting
- Block and mute system
- Report flow (all categories, queue-based review)
- Build code sharing in chat
- Item linking (basic — name + rarity color)
- Chat panel with tabs, opacity, resize
- Friend list integration (online status, right-click actions)
- `/lfg` command (basic)
- New account level gates (level 5 whisper, level 10 trade/global)

### Post-MVP

- Trade chat, Global chat (enabled at level 10)
- Faction chat (Exalted gate)
- Automated toxicity detection (Tier 1-3)
- Shadow-muting system
- Customizable profanity filter (Strict/Standard/Off)
- Advanced LFG with structured UI
- Guild recruitment formatted messages
- Mention system (`@player`, `@party`, `@guild`)
- Merged/custom channel tabs
- Chat search
- Appear offline status
- Timestamps (relative and absolute)
- Appeal process (online form at launch, in-game at post-MVP)

---

## Related Pages

- [[UI Design]] — Hub Panel chat element, text color language, semantic colors
- [[Guild System]] — Guild chat, war council chat, ranks and permissions, recruitment board
- [[Factions System]] — Faction chat access at Exalted tier, faction channel names
- [[Social Hubs]] — Hub capacity, player lists, emote commands, guild recruitment board
- [[Creator Ecosystem]] — Build sharing, build codes, build browsing
- [[Trade System]] — Trade chat context, marketplace integration
- [[Account & Security System]] — Age verification for profanity filter, account-level privacy settings
- [[MVP Design]] — Feature phasing and launch scope
