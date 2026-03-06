---
tags:
  - gdd
  - ui
  - notifications
  - mail
---

# Notification & Mail System

The async information delivery pipeline for Stormbound Online. This system handles every event that needs to reach a player -- whether they are online, in combat, or offline.

---

## Design Principles

1. **Signal, Not Noise** -- notifications exist to inform decisions, not to create anxiety. Default settings suppress low-value alerts.
2. **Respect Player State** -- a player in a dungeon boss fight does not need to know their gem sold. Delivery timing adapts to context.
3. **No Transfer Channel** -- mail attachments are system-generated only. Player-to-player mail cannot carry items or gold. This preserves the [[Trade System]]'s NPC-mediated economy.
4. **Offline Continuity** -- the game world does not stop when a player logs off. The notification queue ensures they return informed, not overwhelmed.

---

## Notification Categories

### Economic

| Trigger | Message Example | Priority |
|---|---|---|
| Marketplace gem sold | "Your Rare Entropy gem sold for 1,200g (1,008g after NPC spread)" | Normal |
| Marketplace gem purchased | "You purchased a Rare Kinetics gem for 950g" | Normal |
| Marketplace listing expired | "Your Epic Void gem listing has expired. Gem returned to inventory" | Normal |
| Commission completed | "Your commissioned Catalyst is ready. Collect from NPC Marketplace" | Normal |
| Price alert triggered | "Rare Tempest gems have dropped below your 800g alert threshold" | Low |

### Social

| Trigger | Message Example | Priority |
|---|---|---|
| Friend request received | "Stormcaller Vex wants to add you as a friend" | Normal |
| Friend request accepted | "Ironheart Kai accepted your friend request" | Low |
| Guild invite received | "The Driftwalkers have invited you to join their guild" | Normal |
| Party invite received | "Ashborn Lira invited you to a party" | Normal |
| Build comment received | "New comment on your build 'Entropy Shredder v3'" | Low |
| Build forked | "Your build 'Void Control' was forked by Tempest Wren" | Low |

### Combat

| Trigger | Message Example | Priority |
|---|---|---|
| Dungeon ready (LFG match found) | "Dungeon group found: Tidecaller Caverns (Hard). Accept?" | Urgent |
| PvP match found | "Arena match found: 3v3 Ranked. Accept?" | Urgent |
| World boss spawning | "World Boss: Stormclaw Leviathan spawning in Calm Shores in 10 minutes" | Normal |

### System

| Trigger | Message Example | Priority |
|---|---|---|
| Maintenance warning | "Server maintenance in 30 minutes. Save your progress" | Urgent |
| Patch notes available | "Patch 1.2.0 notes available. See what changed" | Low |
| Seasonal event start | "The Storm Festival begins! Visit Port Haven for details" | Normal |
| Terms of service update | "Updated Terms of Service. Review required" | Normal |

### Achievement

| Trigger | Message Example | Priority |
|---|---|---|
| Resonance discovered | "New resonance discovered: Kinetics + Entropy -- Shatter Cascade" | Normal |
| Mastery milestone | "Tempest Domain reached Mastery 10!" | Normal |
| Prestige level gained | "Prestige Level 2 unlocked. New cosmetics available" | Normal |
| Faction tier reached | "You are now Honored with The Luminari" | Normal |

---

## Notification Delivery

### Delivery Methods

| Method | Description | Persistence |
|---|---|---|
| **Toast** | Brief pop-up in the corner of the screen. Auto-dismisses after 4 seconds. Non-blocking -- does not interrupt gameplay | Transient |
| **Inbox** | Persistent list in the Notification Panel. Scrollable, filterable by category. Entries persist until read or expired | 30 days |
| **Badge** | Red dot/counter on relevant UI panels (Social tab badge for friend requests, Economy tab badge for sales) | Until acknowledged |

### Priority-Based Delivery

| Priority | Toast | Inbox | Badge | Sound | Additional |
|---|---|---|---|---|---|
| **Urgent** | Yes (flashing border) | Yes | Yes | Alert chime | Screen-edge flash. Cannot be suppressed except by DND |
| **Normal** | Yes | Yes | Yes | Soft chime | Standard delivery |
| **Low** | No | Yes | Counter only | None | Inbox-only by default. Player can promote to Normal per category |

### Context-Aware Suppression

The system suppresses non-urgent notifications based on player state:

| Player State | Urgent | Normal | Low |
|---|---|---|---|
| In hub (idle) | Immediate | Immediate | Inbox only |
| In hub (in menu/panel) | Immediate | Queued (delivered on panel close) | Inbox only |
| In combat | Immediate | Queued (delivered after combat) | Inbox only |
| In dungeon (between fights) | Immediate | Immediate | Inbox only |
| Appear Offline | Immediate | Inbox only | Inbox only |
| AFK | Immediate | Inbox + queued for return | Inbox only |

"Queued" notifications are delivered as a batch when the suppression condition ends, with a brief summary toast: "3 notifications while you were busy."

---

## Notification Inbox

### Layout

The Notification Inbox is accessible from the Hub Panel's Notifications element (see [[UI Design]]).

| Element | Description |
|---|---|
| **Category Tabs** | All, Economic, Social, Combat, System, Achievement |
| **Entry List** | Scrollable. Each entry shows: icon, category color, message text, timestamp, read/unread indicator |
| **Actions** | Mark as read, mark all as read, delete, clear all read |
| **Filter** | By category, by priority, by read/unread |

### Inbox Rules

| Rule | Detail |
|---|---|
| Max inbox size | 200 entries |
| Entry expiry | 30 days (auto-deleted) |
| Overflow behavior | Oldest read entries deleted first. Unread entries are protected until the 200 cap forces eviction (oldest unread evicted last) |
| Urgent entries | Never auto-deleted. Must be manually dismissed |

---

## Mail System

Asynchronous message and attachment delivery. Mail is distinct from notifications -- it carries content that the player must actively retrieve.

### Mail Sources

Mail is **system-only**. Players cannot send mail to each other. All mail originates from game systems or NPCs.

| Source | Content | Has Attachment? |
|---|---|---|
| **NPC Marketplace** | Transaction confirmation: gold from gem sale, purchased gem delivery | Yes (gold or gem) |
| **NPC Commission Board** | Completed commission: crafted item delivery, fee payment to crafter | Yes (item or gold) |
| **System Rewards** | Login rewards (if implemented), event rewards, compensation | Yes (items, gold, cosmetics) |
| **Guild Announcements** | Guild leader broadcasts (text only, no attachments) | No |
| **GM Communications** | Customer support responses, policy notices | No |
| **Faction Rewards** | Faction event participation rewards, weekly war rewards | Yes (items, cosmetics) |

### Mail Interface

| Element | Description |
|---|---|
| **Mail List** | Scrollable list. Each entry: sender name (NPC/system), subject line, attachment indicator, timestamp, read/unread |
| **Mail Detail** | Full message body. Attachment display with "Claim" button |
| **Claim All** | Bulk-claim all attachments from all unread mail (deposits to inventory/wallet) |

### Mail Rules

| Rule | Detail |
|---|---|
| Max mailbox size | 100 messages |
| Mail with attachments | Expires after **30 days**. Unclaimed attachments are lost. Warning notification at 7 days and 1 day remaining |
| Mail without attachments | Expires after **14 days** |
| Overflow behavior | New mail cannot be delivered if mailbox is full. Sender (NPC) holds the mail and player receives an urgent notification: "Your mailbox is full. Claim or delete mail to receive new items" |
| Attachment claim | Items go to inventory (must have space). Gold goes to wallet (no cap). If inventory is full, claim fails with message "Free up inventory space to claim this item" |

### Why No Player-to-Player Mail

Direct player mail with attachments would create an unrestricted P2P transfer channel, violating the [[Trade System]]'s core design principle: "All trade flows through NPCs." Even text-only player mail is excluded from MVP to avoid:

- Spam and harassment vectors beyond whispers
- Social pressure to "gift" items via a future attachment system
- Infrastructure cost for a feature that whispers and party chat already cover for real-time communication

If player-to-player text mail is added post-launch, it must be text-only with no attachment capability, rate-limited, and subject to the block system.

---

## Notification Preferences

Players configure notification behavior in Settings > Notifications.

### Per-Category Settings

| Category | Toggle Options | Default |
|---|---|---|
| Economic | On / Inbox Only / Off | On |
| Social | On / Inbox Only / Off | On |
| Combat | On / Inbox Only / Off | On (cannot set Off for Urgent combat notifications) |
| System | On / Inbox Only / Off | On (cannot set Off for maintenance warnings) |
| Achievement | On / Inbox Only / Off | On |

### Sound Settings

| Category | Sound Toggle | Default |
|---|---|---|
| Economic | On / Off | On |
| Social | On / Off | On |
| Combat | On / Off | On |
| System | On / Off | On |
| Achievement | On / Off | Off |

### Do Not Disturb Mode

- **Effect:** Suppresses all toast notifications and sounds except Urgent priority.
- **Activation:** Manual toggle in Settings or via a quick-access button on the Hub Panel.
- **Auto-DND:** Optionally enabled during dungeon runs (Settings > Notifications > Auto-DND in Dungeons). Disabled by default.
- **Inbox:** All suppressed notifications still appear in the inbox. Nothing is lost.

### Sensible Defaults Philosophy

The default configuration is designed so a new player does not feel overwhelmed:

| Principle | Implementation |
|---|---|
| No spam on first login | Achievement and price alert notifications default to inbox-only until the player has been active for 24 hours |
| Combat never missed | Dungeon ready and PvP match found are always Urgent and cannot be downgraded |
| Economy feedback | Marketplace sale notifications are Normal priority -- a player should know when their gem sells |
| Social is opt-in escalation | Friend requests are Normal. Build comments are Low. Players who engage with the creator ecosystem can promote build comments to Normal |

---

## Offline Notifications

### Queue Behavior

When a player is offline, notifications accumulate in a server-side queue.

| Rule | Detail |
|---|---|
| Queue cap | 50 notifications |
| Overflow eviction | Low priority evicted first, then Normal. Urgent never evicted |
| Within same priority | Oldest evicted first |
| Duplicate suppression | Repeated notifications of the same type are collapsed: "5 gems sold" instead of 5 separate entries |

### Login Summary

On login, if the player has offline notifications, a summary toast appears:

```
While you were away:
  3 gems sold for 2,400g total
  2 friend requests
  1 guild announcement
  Stormclaw Leviathan was defeated (world boss)
```

The summary groups notifications by category and shows counts + key details. The full notifications are available in the inbox.

### Login Summary Rules

| Rule | Detail |
|---|---|
| Summary display | Toast that persists until dismissed (does not auto-dismiss) |
| Threshold | Summary only appears if 3+ offline notifications exist. Fewer than 3 are delivered as individual toasts |
| Attachment mail | Not included in summary toast. Delivered to mailbox separately with badge on Mail icon |

---

## Integration Points

| System | Integration |
|---|---|
| [[UI Design]] | Notifications element in Hub Panel. Toast rendering follows UI Design color language (system messages in gray, social in white, economic in gold) |
| [[Trade System]] | Marketplace sale/purchase/expiry events. Commission completion events. Price alert triggers |
| [[Creator Ecosystem]] | Build comment and build fork events |
| [[Social Hubs]] | Notification badge visible on Hub Panel in all hubs |
| [[Social & Friends System]] | Friend request, party invite, LFG match found events |
| [[Guild System]] | Guild invite, guild announcement events |
| [[PvP Structure]] | PvP match found event |
| [[PvE Structure]] | World boss spawn event |
| [[Factions System]] | Faction tier reached, faction event reward events |

---

## MVP Scope

For MVP (see [[MVP Design]]), the following notification and mail features are included:

| Feature | MVP? | Notes |
|---|---|---|
| Toast notifications | Yes | Core feedback mechanism |
| Notification inbox | Yes | Required for async awareness |
| Priority system (Urgent/Normal/Low) | Yes | Required for context-appropriate delivery |
| Notification preferences | Yes | Per-category toggle + sound |
| Mail system (NPC/system only) | Yes | Required for marketplace gold/gem delivery |
| Mail attachments | Yes | Required for marketplace transactions |
| Offline notification queue | Yes | Required for async game events |
| Login summary | Yes | Quality of life |
| Do Not Disturb mode | Post-MVP | Nice to have, not critical |
| Auto-DND in dungeons | Post-MVP | Depends on DND |
| Context-aware suppression | Post-MVP | Requires player state tracking integration |
| Price alerts | Post-MVP | Marketplace maturity feature |

---

## Related Pages

- [[UI Design]]
- [[Trade System]]
- [[Creator Ecosystem]]
- [[Social & Friends System]]
- [[Social Hubs]]
- [[Guild System]]
- [[PvP Structure]]
- [[PvE Structure]]
- [[Factions System]]
- [[MVP Design]]
