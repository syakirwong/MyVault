---
tags:
  - gdd
  - meta
  - communication
  - live-ops
  - patches
---

# Patch & Communication System

This document defines how game updates, balance changes, and live service events are communicated to players. Communication is a design system — not an afterthought. How players learn about changes affects whether those changes are accepted, resisted, or ignored.

For balance philosophy and patch cadence, see [[Meta Management]].
For the combat systems affected by balance patches, see [[Combat System]].
For gem and domain balance, see [[Gem Identity System]] and [[Gem System Math]].
For localization of player-facing communications, see [[Localization Plan]].

---

## Design Principles

1. **In-game first.** The primary communication channel is always inside the game. Players should never need to leave the client to understand what changed.
2. **No surprise nerfs.** Balance reductions are announced before they ship. Players who invested in a build deserve time to plan.
3. **Numbers, not adjectives.** "Reduced" means nothing. "Reduced from 35% to 28%" means everything. All balance changes include old and new values.
4. **Explain the why.** Every significant change includes designer intent. Players who understand the reasoning tolerate changes they disagree with.
5. **Respect investment.** When a change invalidates player effort, acknowledge it explicitly and describe compensation or alternatives.

---

## Communication Channels

### Channel Hierarchy

| Channel | Audience | Content Types | Update Speed |
|---------|----------|---------------|-------------|
| **In-game News Panel** | All active players | Patch notes, events, maintenance, seasonal previews | Immediate on publish |
| **In-game Login Message** | All players on next login | Critical announcements, emergency hotfixes, server status | Immediate on publish |
| **Official Website / Blog** | All players + prospective players | Full patch notes, developer commentary, retrospectives, roadmap | Same day as in-game |
| **Community Discord / Forum** | Engaged community | Discussion threads, developer responses, feedback collection | Same day as in-game |
| **Social Media** | Broad audience | Summaries, teasers, highlights, community spotlights | Same day or day after |

**Rule:** No information appears on social media or external channels before it appears in-game. The game client is always the first and most complete source.

### Channel Usage by Event Type

| Event | In-Game News | Login Message | Website | Discord/Forum | Social |
|-------|-------------|---------------|---------|---------------|--------|
| Monthly balance patch | Full notes | Summary + link | Full notes + commentary | Discussion thread | Highlights |
| Seasonal update | Full notes | Summary + link | Full notes + preview article | Discussion thread + AMA | Trailer/teaser |
| Emergency hotfix | Immediate note | Alert banner | Post-mortem within 48h | Incident thread | Brief status |
| Scheduled maintenance | Advance notice | Countdown timer | Schedule post | Advance notice | Brief notice |
| Upcoming nerf (advance notice) | Dedicated announcement | Alert if major | Blog post with reasoning | Discussion thread | Summary |
| Monthly Meta Report | Full report | Notification badge | Archived on website | Discussion thread | Infographic |

---

## Patch Notes Format

### Standard Structure

Every patch note follows this template:

```
============================================
STORMBOUND ONLINE — PATCH {version}
{date}
============================================

OVERVIEW
--------
{2-3 sentence summary of the patch's theme and major changes}

BALANCE
-------
[Domain / Gem / Modifier changes]

  {Item Name}
    What changed: {description}
    Old value: {number/behavior}
    New value: {number/behavior}
    Why: {1-2 sentence designer intent}

  {Item Name}
    ...

CONTENT
-------
[New dungeons, quests, NPCs, regions]

  {Content Name}
    {Description of what was added}

ECONOMY
-------
[Drop rates, prices, crafting changes]

  {System/Item}
    What changed: {description}
    Old value: {number}
    New value: {number}
    Why: {intent}

BUG FIXES
---------
  - {Bug description} — Fixed.
  - {Bug description} — Fixed. {Additional context if the bug was widely reported}

QUALITY OF LIFE
---------------
  - {Improvement description}
  - {Improvement description}

KNOWN ISSUES
------------
  - {Issue description} — {Status: investigating / fix planned for next patch}

DESIGNER COMMENTARY
-------------------
{2-4 paragraphs from the design team on the patch's biggest changes,
the data that drove decisions, and what they're watching going forward}
============================================
```

### Balance Change Format — Detail

Every balance change includes four elements:

| Element | Required | Example |
|---------|----------|---------|
| What changed | Yes | "Combo Finisher modifier bonus damage" |
| Old value | Yes | "35%" |
| New value | Yes | "28%" |
| Why | Yes | "Combo Finisher exceeded 40% pick rate and appeared in 7 of the top 10 PvP loadouts. This adjustment brings it in line with other modifier options without removing its identity as a burst finisher." |

**Rules:**
- Never use vague language ("adjusted," "tuned," "updated") without numbers.
- If a change affects PvP and PvE differently (per [[Meta Management#Tier 3]]), list both values.
- If a change is a revert of a previous change, reference the original patch version.
- Group related changes under the affected domain or system, not scattered across categories.

### Designer Commentary Guidelines

Designer commentary accompanies every monthly patch and seasonal update. It is written by the balance team lead or game director.

**Must include:**
- What data triggered the changes (reference specific metrics from [[Meta Management#Data Pipeline]])
- What the team considered but decided against (and why)
- What they are watching for next month
- Acknowledgment of community feedback that influenced decisions

**Must not include:**
- Apologies for making changes (balance is expected)
- Promises about future specific changes (only directions and intentions)
- Dismissal of community concerns

---

## Patch Cadence and Communication Timeline

### Monthly Balance Patch

Cadence defined in [[Meta Management#Tier 3 Data-Driven Balance Patches]]. Communication wraps around that cadence.

| Timeline | Action | Channel |
|----------|--------|---------|
| **Day -14** | Internal: Patch scope finalized. Balance changes locked. | Internal only |
| **Day -7** | **Advance Notice:** Publish upcoming balance changes (especially nerfs). Full old/new values. Designer reasoning. | In-game News Panel, Website, Discord |
| **Day -7 to -1** | Community discussion period. Team monitors feedback but changes are locked unless critical flaw identified. | Discord/Forum |
| **Day 0** | **Patch deployed.** Full patch notes published simultaneously across all channels. | All channels |
| **Day 0** | Login message displays patch summary with link to full notes. | In-game Login Message |
| **Day +1 to +3** | Monitor for emergent issues. Publish "Known Issues" addendum if needed. | In-game News Panel, Discord |
| **Day +7** | Internal: Begin collecting data for next month's patch. | Internal only |

### Seasonal Update (Quarterly)

Seasonal updates are the largest communication events. They combine balance changes, new content, meta shifts, and reward track reveals.

| Timeline | Action | Channel |
|----------|--------|---------|
| **Week -4** | Internal: Seasonal content complete. Balance finalized. | Internal only |
| **Week -2** | **Season Preview:** Publish detailed preview of upcoming season. | All channels |
| **Week -1** | **Advance Balance Notice:** Publish all balance changes following the same Day -7 rules as monthly patches. | In-game News, Website, Discord |
| **Day 0** | **Season launches.** Full patch notes. Season-specific login message. In-game event banner. | All channels |
| **Week +1 (of new season)** | **Season Retrospective** for the ended season: meta statistics, community highlights. | Website, In-game News, Discord |

#### Season Preview Content (Week -2)

| Section | Content |
|---------|---------|
| New content | Regions, dungeons, quests, NPCs being added |
| Meta shifts | Which resonances are rotating in/out, domain amplification, alignment season bonus |
| Reward track | Season pass rewards preview (cosmetic track per [[Behavioral Identity#Cosmetic Economy]]) |
| Competitive | Ranked ladder reset details, new PvP rules or modes |
| Quality of life | Major UX improvements shipping with the season |

#### Season Retrospective Content (Week +1 After Season End)

| Section | Content |
|---------|---------|
| Meta statistics | Top loadouts (PvP and PvE), domain pick rates, alignment distribution |
| Most popular builds | Top 5 community-published builds by usage |
| Economy health | Gold supply, marketplace volume, crafting participation |
| Community highlights | Top creators, most innovative builds, notable PvP moments |
| What we learned | Design team reflection on what worked and what didn't |

### Emergency Hotfix

Hotfix triggers and protocol defined in [[Meta Management#Tier 5 Emergency Intervention]]. Communication wraps around the technical response.

| Timeline | Action | Channel |
|----------|--------|---------|
| **Hour 0** | Issue identified. Internal escalation. | Internal only |
| **Hour 0-2** | If issue is publicly visible: immediate acknowledgment. "We are aware of [issue]. Investigating." | Login Message banner, Discord pinned post |
| **Hour 0-48** | Fix developed, tested, deployed (per [[Meta Management#Tier 5]]). | Internal |
| **Deploy** | **Immediate patch note:** What was broken, what we changed, why. Minimal format — can skip full template. | In-game News, Discord, Website |
| **Within 48h** | **Post-mortem:** Full explanation. Root cause, impact scope, fix details, prevention measures. If players were negatively affected, describe compensation. | Website, In-game News, Discord |

**Hotfix patch note format (minimal):**

```
============================================
HOTFIX — {version}
{date} {time} UTC
============================================

ISSUE: {What was broken, in player-facing terms}
IMPACT: {Who was affected and how}
FIX: {What we changed}
COMPENSATION: {If applicable — refunded materials, restored progress, etc.}
STATUS: Resolved. Monitoring for recurrence.

Full post-mortem will follow within 48 hours.
============================================
```

### Scheduled Maintenance

| Timeline | Action | Channel |
|----------|--------|---------|
| **-24 hours** | Maintenance announcement: date, time (UTC + major timezone conversions), estimated duration, what's being worked on (if disclosable). | In-game News, Login Message, Discord |
| **-1 hour** | In-game countdown timer appears in hub UI. Reminder notification. | In-game |
| **-15 minutes** | Final warning. Players in active combat receive a "maintenance in 15 minutes" overlay. No new PvP matches can start. | In-game |
| **During** | Website maintenance page with status updates. Discord status updates every 30 minutes if maintenance exceeds estimate. | Website, Discord |
| **Complete** | "Maintenance complete" notification. If anything changed during maintenance, publish a brief note. | All channels |

---

## Advance Notice for Nerfs

This section operationalizes [[Meta Management]]'s philosophy of "never gut, always redirect" and the transparency principle.

### The 7-Day Rule

All nerfs (value reductions to gems, domains, modifiers, resonances, or systemic rules) are published at least 7 calendar days before they go live.

**Exceptions:**
- Emergency hotfixes for game-breaking exploits (see [[Meta Management#Tier 5]]). These ship immediately but must include post-mortem communication.
- Bug fixes that happen to reduce power are not nerfs and do not require advance notice (but should be clearly labeled as bug fixes, not balance changes, to avoid confusion).

### Advance Nerf Notice Format

```
============================================
UPCOMING BALANCE CHANGES — Effective {date}
============================================

The following changes will go live with Patch {version} on {date}.

NERFS
-----

  {Item/System Name}
    Current value: {number}
    New value: {number}
    Change: {-X% or -X points}
    Reason: {Why this is being changed. Reference data: win rate, pick rate,
             or other metric from Meta Management thresholds.}
    Alternatives: {What builds or strategies players should consider if this
                   change affects their current loadout.}

  {Item/System Name}
    ...

BUFFS
-----
{Buffs are also previewed for context — players need to see the full picture.}

  {Item/System Name}
    Current value: {number}
    New value: {number}
    Change: {+X% or +X points}
    Reason: {Why this is being buffed.}

CONTEXT
-------
{2-3 paragraphs explaining the overall balance direction. What data drove
these decisions. What the team is trying to achieve. What they're watching.}
============================================
```

### Why Advance Notice Matters

| Without Advance Notice | With Advance Notice |
|------------------------|---------------------|
| Players feel ambushed | Players feel respected |
| "My build was destroyed overnight" | "I had a week to prepare" |
| Community rage peaks on patch day | Community processes changes before they hit |
| Feedback arrives too late to matter | Feedback can catch errors before deployment |
| Trust erodes over time | Trust compounds over time |

### What "Alternatives" Means

The `Alternatives` field in nerf notices is not optional. When a build or strategy is being weakened, the notice must suggest:
- Which other domains, modifiers, or resonances fill a similar role
- Whether the nerf opens up previously non-viable options
- Specific loadout directions that benefit from the surrounding buff/nerf context

This respects the player's investment. They chose a build path. The notice says: "We're changing the landscape, and here's how to navigate it."

---

## Monthly Meta Report Communication

The Monthly Meta Report (defined in [[Meta Management#Monthly Meta Report Public]]) is a communication event.

### Publication Schedule

| Timeline | Action |
|----------|--------|
| 1st of each month | Report published |
| Covers | Previous calendar month's data |
| Channels | In-game News Panel (summary + link), Website (full report), Discord (discussion thread) |

### Report Sections (Player-Facing)

| Section | Content |
|---------|---------|
| Top 10 PvP loadouts | By pick rate and win rate. Archetype names, domain combinations. |
| Top 10 PvE loadouts | By dungeon clear efficiency. |
| Domain pick rate | Pie chart or bar chart showing all 8 domains. |
| Alignment distribution | Current player split across Radiant, Corrupted, Unbound. |
| Rising builds | Biggest win rate or pick rate increases. |
| Falling builds | Biggest decreases. |
| Rotation preview | Next month's Challenge Mode themes, Mythic rotation, Solo Trial focus. |
| Economy snapshot | Marketplace volume trends, most-traded gem domains. |

### Design Intent

The Monthly Meta Report serves two purposes:
1. **Player utility** — Strategic information for build planning and market decisions.
2. **Transparency signal** — Proof that the development team is watching the same data players care about. This builds trust even when no changes are made.

---

## Community Feedback Loop

### Feedback Collection

| Method | What It Captures | Frequency |
|--------|-----------------|-----------|
| **In-game feedback form** | Bug reports, balance complaints, feature requests. Accessible from Settings menu. Includes optional screenshot and build code attachment. | Always available |
| **Post-match survey** (PvP) | "Did this match feel fair?" (1-5 scale). Optional free-text. Appears after 1 in 10 PvP matches (randomized, not after every match). | Ongoing, sampled |
| **Seasonal survey** | Structured questionnaire at season end. Covers satisfaction with balance, content, economy, communication. 10-15 questions. Completion reward: small cosmetic (chat flair or profile banner). | Quarterly |
| **Community forums / Discord** | Open discussion. Threads per topic (balance, bugs, suggestions, praise). Upvote system for prioritization. | Always available |
| **Bug tracker (public)** | Known issues list with status tracking. Players can subscribe to issues for updates. | Always available |

### Feedback Processing

| Stage | Owner | Action |
|-------|-------|--------|
| **Collection** | Community team | Aggregate feedback by category. Flag high-volume topics. |
| **Triage** | Community team + design lead | Categorize: bug / balance / feature / QoL. Assign priority. |
| **Response** | Design team | Top-voted issues receive an official response within 2 weeks. Response states: acknowledged, investigating, planned, won't fix (with reasoning), or already addressed. |
| **Action** | Development team | Issues that enter the development pipeline are tracked on the public roadmap. |
| **Closure** | Community team | When an issue ships a fix or feature, the original thread/report is updated with the patch version and link to notes. |

### Public Roadmap

A living document (website page) showing:

| Column | Content |
|--------|---------|
| **Shipped** | Recently completed features and fixes with patch version |
| **In Progress** | Currently in development. No dates, only "this season" or "next season" granularity |
| **Planned** | Accepted into the pipeline. No timeline commitment |
| **Considering** | Community-requested features under evaluation. Includes design team's current thinking |

**Rules:**
- The roadmap never promises dates for unshipped features.
- Items move between columns publicly — players can see progress.
- Items removed from the roadmap include a brief explanation of why.
- The roadmap is updated at minimum with every monthly patch.

### Transparency Commitments

| Commitment | Cadence |
|------------|---------|
| Respond to top 5 community-voted issues | Monthly (in Designer Commentary or dedicated post) |
| Publish data that drove balance decisions | Every balance patch (in patch notes) |
| Credit community feedback when it influenced a change | In patch notes: "Community feedback highlighted that..." |
| Explain rejected feedback | Quarterly (in seasonal retrospective or dedicated post) |

---

## In-Game News Panel

### UI Specification

**Location:** Accessible from the hub UI. A "News" button in the top navigation bar of the hub panel (see [[UI Design]]).

**Notification badge:** Unread news items display a badge (number) on the News button. Badge clears when the player opens the panel and scrolls past new items.

**Layout:**

```
+------------------------------------------+
| NEWS                              [Close] |
|------------------------------------------|
| [Patch Notes] [Events] [Community] [All] |
|------------------------------------------|
| ! PATCH 1.3.2 — Balance Update    NEW   |
|   March 6, 2026                          |
|   Modifier adjustments, bug fixes...     |
|------------------------------------------|
|   Season 4 Preview                       |
|   February 28, 2026                      |
|   New resonances, domain amplification...|
|------------------------------------------|
|   Maintenance Complete                   |
|   February 25, 2026                      |
|   Server maintenance concluded...        |
|------------------------------------------|
|   Monthly Meta Report — February         |
|   February 1, 2026                       |
|   Top loadouts, domain distribution...   |
|------------------------------------------|
|              [Load More]                 |
+------------------------------------------+
```

### Panel Features

| Feature | Description |
|---------|-------------|
| **Categories** | Patch Notes, Events, Community, Maintenance. "All" shows everything. |
| **NEW badge** | Unread posts marked with `NEW` tag. Clears on view. |
| **Critical banner** | Emergency hotfixes and server issues display as a colored banner at the top of the panel (and optionally as an overlay on the hub). |
| **Expandable posts** | Clicking a post expands it in-panel. Short posts display fully. Long posts (full patch notes) show a summary with "Read Full Notes" link that opens a scrollable detail view. |
| **Archive** | All past posts accessible via scroll or "Load More." Minimum 6 months of history. |
| **Search** | Text search across all archived posts. |
| **Link to external** | Posts can include a "View on Website" link for the full-format version with images and embedded charts. |
| **Localized** | All news panel content is localized per [[Localization Plan]]. Language follows client language setting. |

### Login Message

Separate from the News Panel. Appears as a modal on login for critical announcements only.

**Triggers for login message:**
- Patch deployed since last login
- Emergency hotfix deployed since last login
- Scheduled maintenance within 2 hours
- Season launch or season end
- Account-specific alerts (compensation applied, build affected by balance change)

**Format:**
- Title + 2-3 sentence summary
- "View Details" button (opens News Panel to relevant post)
- "Dismiss" button
- Auto-dismisses after 10 seconds if player takes no action (does not re-show)

**Rules:**
- Maximum 1 login message per session. If multiple events qualify, show the highest priority.
- Login messages are never used for marketing, promotions, or cash shop announcements.

### Priority Order for Login Messages

| Priority | Event |
|----------|-------|
| 1 (highest) | Emergency hotfix / server issue |
| 2 | Account-specific alert (compensation, build impact) |
| 3 | Patch deployed |
| 4 | Season launch/end |
| 5 | Scheduled maintenance warning |

---

## Communication Quality Standards

### Tone

- **Professional but human.** Not corporate-speak. Not memes. The voice is the design team talking to players who care about the game.
- **Direct.** Say what changed and why. Do not hedge with "we feel" or "we believe" — state the data and the decision.
- **Respectful of player investment.** Never dismiss a nerf as minor. If it matters enough to change, it matters enough to explain.

### Consistency

- Patch notes use the same format every time. Players should know where to find balance changes, bug fixes, and designer commentary by muscle memory.
- Terminology is consistent across all channels. If the in-game patch note says "Combo Finisher bonus damage reduced from 35% to 28%," the Discord summary, website post, and social media all use the same numbers and the same name.
- All communications reference the same version number.

### Timeliness

| Event | Communication Deadline |
|-------|----------------------|
| Monthly patch | Advance notice 7 days before. Full notes on deploy. |
| Seasonal update | Preview 2 weeks before. Full notes on deploy. |
| Emergency hotfix | Acknowledgment within 2 hours of awareness. Full note on deploy. Post-mortem within 48 hours. |
| Scheduled maintenance | 24 hours advance notice. |
| Monthly Meta Report | Published by the 3rd of each month. |

---

## Related Pages

- [[Meta Management]] — Balance philosophy, patch cadence, data pipeline
- [[Combat System]] — Systems affected by balance patches
- [[Gem Identity System]] — Gem domains, resonances, archetype naming
- [[Gem System Math]] — Formulas and power budget referenced in balance changes
- [[Creator Ecosystem]] — Community content and build sharing
- [[UI Design]] — Hub panel layout, notification systems
- [[Localization Plan]] — Localization of patch notes and player communications
- [[Economy Model]] — Economy metrics referenced in Meta Reports
