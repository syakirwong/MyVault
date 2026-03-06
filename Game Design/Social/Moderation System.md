---
tags:
  - gdd
  - social
  - moderation
  - safety
---

# Moderation System

The backend moderation infrastructure for Stormbound Online. This document defines the full lifecycle of player reports, automated detection, moderator investigation, enforcement actions, appeals, and data retention. It extends the player-facing moderation tools defined in [[Chat & Communication System#Moderation Tools]] with the systems-level design required to operate moderation at scale.

---

## Design Philosophy

Moderation exists to protect the player experience, not to punish. Every system described here optimizes for three properties in order of priority:

1. **Speed** — harmful behavior is detected and acted on before it damages community trust
2. **Accuracy** — false positives erode player confidence in the system more than false negatives
3. **Transparency** — players understand the rules, the consequences, and have recourse when they believe an error occurred

Moderation is not optional infrastructure. It is as critical as combat balance or server stability. An unmoderated game does not have a toxicity problem — it has a retention problem.

---

## Report Categories

| Category | Description | Severity | Examples |
|----------|-------------|----------|----------|
| **Harassment** | Targeted abuse, threats, sustained bullying, stalking behavior | High | Repeated hostile whispers, following a player between zones to grief, targeted insults |
| **Hate Speech** | Slurs, discrimination, extremism, identity-based attacks | Critical | Racial slurs, homophobic language, extremist recruitment |
| **Cheating / Exploiting** | Using bugs, external tools, or unintended mechanics for unfair advantage | High | Damage hacks, speed manipulation, gem duplication exploits, action timing manipulation |
| **Botting** | Automated play without human input | High | Repetitive identical combat sequences, 24-hour continuous play patterns, inhuman reaction consistency |
| **Inappropriate Name / Content** | Offensive character names, guild names, or build code names | Medium | Slur-containing names, sexual references, impersonation of staff |
| **Spam** | Repetitive messages, advertising, unsolicited promotion | Medium | Gold selling advertisements, repeated trade listings, chat flooding |
| **Account Selling / Sharing** | Trading real money for in-game accounts, items, or services | Medium | "WTS account" messages, suspicious login pattern changes (geography, device) |
| **Griefing** | Intentional actions to harm teammates or disrupt gameplay | High | Intentional throwing in ranked PvP (repeated zero-action commits), blocking dungeon boss progression (refusing to commit actions in group PvE), trade scamming |

---

## Report Submission

### In-Game Report Flow

1. **Initiate:** Right-click player name (in chat, combat log, party panel, or player inspect) and select "Report"
2. **Category:** Select one report category from the list above
3. **Details (optional):** Free-text description field (200 characters max)
4. **Evidence (automatic):** System captures contextual evidence based on category:

| Category | Auto-Captured Evidence |
|----------|----------------------|
| Harassment, Hate Speech, Spam | Reported message + 10 surrounding messages (per [[Chat & Communication System#Report Flow]]) |
| Cheating / Exploiting | Last 3 combat log excerpts involving the reported player (see [[Combat Logs System]]) |
| Botting | Last 10 combat sequences of the reported player (action pattern data) |
| Griefing | Last 5 rounds of shared combat log (group PvE) or ranked match log (PvP) |
| Inappropriate Name | Current character name, guild name, and build code names |
| Account Selling | Chat context + account metadata (login patterns, not shared with reporter) |

5. **Confirmation:** Reporter receives: "Report submitted. You will be notified when it has been reviewed."
6. **Feedback (within 24 hours):** System message: "Your report has been reviewed. Action has been taken." No specifics about the action are disclosed.

### Rate Limits

| Constraint | Limit | Rationale |
|------------|-------|-----------|
| Reports per player per day | 5 | Prevents report spam and weaponized reporting |
| Reports against same target per day | 2 | Prevents targeted harassment via report system |
| Minimum account level to report | 5 | Prevents throwaway accounts from flooding the queue |
| Minimum time between reports | 5 minutes | Prevents rage-reporting in rapid succession |

### Trusted Reporter System

- Players whose reports result in confirmed action 90%+ of the time over their last 20 reports earn **Trusted Reporter** status
- Trusted Reporter reports receive +2 priority score in the investigation queue
- Status is revoked if accuracy drops below 70% over a rolling 20-report window
- Trusted Reporters are not notified of their status (prevents gaming the system)
- Trusted Reporters retain the same daily report limit (5) — quality over quantity

---

## Automated Detection

Automated systems operate continuously and independently of player reports. They feed into the same investigation queue with appropriate priority scoring.

### Chat Toxicity Scoring

Integrates with [[Chat & Communication System#Automated Toxicity Detection]].

| Tier | Confidence | Automated Action | Queue Action |
|------|-----------|-----------------|--------------|
| **Tier 1** (Low) | Pattern match, ambiguous context | Message delivered. Flagged for review | Added to queue at Low priority |
| **Tier 2** (Medium) | Heuristic match, likely violation | Message delivered with private warning to sender | Added to queue at Medium priority |
| **Tier 3** (High) | High-confidence slur, threat, or hate speech | Message blocked. Sender warned | Added to queue at High priority for human verification |

False positive rate target: below 2% for Tier 3 blocks. All Tier 2 and Tier 3 actions are logged and available for human review.

### Combat Pattern Analysis (Botting Detection)

| Signal | Detection Method | Threshold |
|--------|-----------------|-----------|
| **Repetitive sequences** | Compare last 20 combat commits for identical action ordering | 15+ identical sequences out of 20 |
| **Inhuman consistency** | Measure commit timing variance across rounds | Standard deviation < 0.1 seconds over 50 rounds |
| **24-hour play** | Continuous session tracking | 16+ hours continuous play without AFK state |
| **Zero adaptation** | Measure response to opponent behavior changes | No commit variation despite opponent strategy shifts over 30+ rounds |

Botting detection flags are High priority in the queue. Automated action: none (false positive risk too high for automated bans). Human review required.

### Speed Hacking Detection (Server-Side Timing Validation)

All combat phase timing is validated server-side. The client reports actions; the server enforces phase durations.

| Validation | Method | Action on Violation |
|------------|--------|-------------------|
| **Commit phase duration** | Server enforces 8-10 second commit window. Actions received before 0.5 seconds are flagged | Flag + log. 3 flags in one match = automatic match void |
| **Action execution rate** | Server tracks time between action submissions | Actions faster than server tick rate are rejected |
| **State desync** | Server compares client-reported state vs authoritative server state | Desync beyond threshold = client resync forced. Repeated desync = flag for investigation |

Speed hacking detection is **fully automated and enforced in real-time**. No human review required for in-match enforcement (action rejection, match void). Post-match investigation may follow for repeated offenders.

### Economy Anomaly Detection

| Signal | Detection Method | Threshold |
|--------|-----------------|-----------|
| **Gold flow spike** | Track gold received per hour vs player level and content tier | >300% of expected gold/hour for content tier |
| **Gem transfer patterns** | Track gem trades between accounts | Repeated one-way transfers of high-value gems between accounts with no reciprocal trades |
| **Marketplace manipulation** | Track buy/sell patterns on NPC Marketplace | Buying all stock of a material and relisting at >200% markup within 1 hour |
| **New account wealth** | Track wealth accumulation for accounts under 48 hours old | >10,000 gold within first 48 hours (expected: ~2,000) |

Economy anomalies are Medium priority by default. Escalated to High if combined with other signals (e.g., gold spike + new account + spam reports).

---

## Investigation Queue

### Priority Scoring

Every report and automated detection entry receives a **priority score** calculated as:

```
Priority = Severity_Base x Report_Volume_Multiplier x History_Multiplier
```

| Factor | Values |
|--------|--------|
| **Severity Base** | Critical: 10, High: 7, Medium: 4, Low: 2 |
| **Report Volume Multiplier** | 1 report: 1.0x, 2-3 reports: 1.5x, 4-5 reports: 2.0x, 6+ reports: 3.0x |
| **History Multiplier** | Clean history: 1.0x, 1 prior action: 1.3x, 2 prior actions: 1.6x, 3+ prior actions: 2.0x |
| **Trusted Reporter Bonus** | +2 to final score per Trusted Reporter report |

### Queue Dashboard (Moderator View)

| Column | Description |
|--------|-------------|
| **Priority Score** | Calculated score, sortable (highest first by default) |
| **Category** | Report category or automated detection type |
| **Target Player** | Reported player name, account age, current status (online/offline/in-combat) |
| **Report Count** | Number of independent reports for this incident |
| **Time in Queue** | Time since first report. Entries older than 24 hours are highlighted |
| **Source** | Player report, automated detection, or both |
| **Quick Actions** | One-click: View Evidence, Spectate Player, Mute, Warn |

### Evidence Package

When a moderator opens an investigation, the system assembles a complete evidence package:

| Evidence Type | Contents | Source |
|---------------|----------|--------|
| **Chat Logs** | All messages from the reported player in the relevant timeframe (reported message + 50 messages context per channel) | [[Chat & Communication System]] server-side logs |
| **Combat Logs** | Full combat logs for flagged matches (all rounds, both perspectives) | [[Combat Logs System]] server-side records |
| **Economic History** | Gold/gem/material transaction log for the past 7 days | Economy system transaction ledger |
| **Behavioral Identity** | Play pattern data: session times, preferred content, social connections, device/IP metadata | [[Behavioral Identity]] system |
| **Report History** | All previous reports filed against this player (with outcomes) | Moderation database |
| **Action History** | All previous moderation actions taken against this player | Moderation database |
| **Account Metadata** | Account creation date, last login, total playtime, character level, faction standings | Account system |

Moderators can filter and search within the evidence package. Chat logs and combat logs support keyword search and timestamp filtering.

---

## Action Escalation

### Standard Escalation Path

| Level | Action | Duration | Trigger |
|-------|--------|----------|---------|
| **1** | **Warning** | N/A | First offense, minor violation (spam, mild toxicity, borderline name) |
| **2** | **Temporary Mute** | 1-24 hours | Chat violations after warning, or moderate first offense |
| **3** | **Temporary Ban** | 1-7 days | Repeated violations after mute, or serious first offense (confirmed exploit use, sustained harassment) |
| **4** | **Extended Ban** | 30 days | Repeated violations after temp ban, or severe offense (botting with economic impact, organized griefing) |
| **5** | **Permanent Ban** | Indefinite | Pattern of behavior showing no reform, or extreme offense (real-life threats, CSAM, organized RMT operation) |

### Severity Shortcuts

Certain offenses bypass the standard escalation path:

| Offense | Immediate Action | Rationale |
|---------|-----------------|-----------|
| **Explicit real-life threats** | Immediate 7-day temporary ban | Player safety. No warning phase |
| **Confirmed exploiting** (with material gain) | Immediate 30-day extended ban + economic rollback | Exploit damage compounds over time. Swift action limits impact |
| **Confirmed speed hacking** | Immediate 30-day extended ban | Automated detection provides high-confidence evidence |
| **Hate speech (Tier 3 confidence)** | Immediate 24-hour mute + queue entry for human review | Automated block prevents further harm while human verifies |
| **Confirmed RMT operation** (organized) | Immediate permanent ban on all associated accounts | Economic integrity. RMT operations have no legitimate use |
| **CSAM or illegal content** | Immediate permanent ban + report to legal/law enforcement | Legal obligation. Zero tolerance |

### Action Notifications

When a moderation action is taken, the affected player receives a system mail:

```
SYSTEM NOTICE ─────────────────────────
Action: Temporary Mute (12 hours)
Reason: Chat violation — harassment
Duration: Expires [timestamp]
Appeal: You may appeal this action via
        Settings > Support > Appeal
─────────────────────────────────────
```

The notification includes: action type, reason category (not specific evidence details), duration, and appeal instructions. It does not include: the reporter's identity, specific detection method details, or exact evidence excerpts (to protect detection systems and reporter safety).

---

## Appeal Process

### Submission

| Method | Availability |
|--------|-------------|
| **In-game** | Settings > Support > Appeal (post-MVP; web form at launch) |
| **Web** | Support portal accessible from account page |

### Appeal Form Fields

| Field | Required | Max Length |
|-------|----------|-----------|
| Player name | Yes | Auto-filled |
| Action being appealed | Yes | Auto-filled from action history |
| Date of action | Yes | Auto-filled |
| Player's statement | Yes | 500 characters |
| Supporting context | No | 500 characters |

### Review Process

| Step | Description | SLA |
|------|-------------|-----|
| **1. Receipt** | System acknowledges appeal: "Your appeal has been received and will be reviewed within 72 hours" | Immediate |
| **2. Assignment** | Appeal assigned to a **senior moderator** who was not involved in the original action | Within 24 hours |
| **3. Review** | Senior moderator reviews: original evidence package, action taken, player's appeal statement, player's full history | — |
| **4. Decision** | One of three outcomes: action upheld, action reversed, action modified (e.g., ban reduced to mute) | Within 72 hours of submission |
| **5. Notification** | Player notified of outcome via system mail and (if applicable) email | Immediate after decision |

### Appeal Constraints

| Rule | Specification |
|------|--------------|
| Appeals per action | 1. No re-appeals on the same incident |
| Appeal eligibility | All actions are appealable except permanent bans for CSAM/illegal content |
| Evidence disclosure | Where possible, the reviewing moderator shares relevant evidence with the player (e.g., "Your account was flagged for sending the following messages..."). Detection method details and reporter identity are never disclosed |
| Reversal effect | If an action is reversed, it is expunged from the player's record. It does not count toward history multiplier in future priority scoring |
| Modified action | If an action is reduced (e.g., 7-day ban -> 3-day ban), the modified action replaces the original in the player's record |

---

## Moderator Tools

### Player Investigation Panel

Accessible from the queue dashboard or by searching any player name:

| Section | Contents |
|---------|----------|
| **Account Overview** | Name, level, faction standings, alignment, account age, total playtime, current status |
| **Action History** | All moderation actions taken against this player, with dates, reasons, and outcomes (including appeals) |
| **Report History (Filed)** | Reports this player has filed against others, with accuracy rate |
| **Report History (Received)** | Reports filed against this player, with categories, dates, and resolution status |
| **Chat History** | Searchable chat log for the player across all channels. Filterable by channel, date, keyword |
| **Combat History** | Recent combat logs. Filterable by mode (PvP/PvE), date, opponent |
| **Economic History** | Transaction log: gold, gems, materials, marketplace activity. Filterable by type, date, value |
| **Social Graph** | Friends list, guild membership, frequent party members, frequent trade partners |
| **Session History** | Login/logout times, session durations, IP addresses, device fingerprints |

### Real-Time Tools

| Tool | Function | Access Level |
|------|----------|-------------|
| **Spectate** | Observe the player's current match in real-time (0-second delay) via [[Spectator Mode]] | Moderator |
| **Temporary Mute** | Immediately mute the player in all chat channels | Moderator |
| **Temporary Ban** | Immediately remove the player from the game for a specified duration (1-7 days) | Moderator |
| **Kick** | Force-disconnect the player from the current session. They can reconnect immediately | Moderator |
| **Watch List** | Mark the player as "watched" — automated detection thresholds are lowered by 30% for this player, and all future reports are auto-escalated to High priority | Moderator |
| **Extended Ban** | Ban for 30 days | Senior Moderator |
| **Permanent Ban** | Permanent account closure | Senior Moderator (requires second Senior Moderator approval) |
| **Economic Rollback** | Reverse specific transactions or reset a player's economic state to a previous snapshot | Senior Moderator |
| **System Mail** | Send a direct system message to the player (not a chat message — appears in their notification/mail panel per [[Notification & Mail System]]) | Moderator |

### Moderator Accountability

| Measure | Specification |
|---------|--------------|
| **Action logging** | Every moderator action is logged with: moderator ID, target player, action type, timestamp, reason/notes |
| **Audit trail** | Senior moderators can review any moderator's action history |
| **Appeal oversight** | If a moderator's actions are reversed on appeal at a rate exceeding 20% over 30 days, their account is flagged for review by a senior moderator |
| **Separation of duties** | The moderator who takes an action cannot review the appeal for that action |
| **Rate limits** | Moderators are limited to 50 actions per day to prevent burnout and rushed decisions |

---

## Data Retention

| Data Type | Retention Period | Post-Retention Action |
|-----------|-----------------|----------------------|
| **Player reports** | 1 year from submission | Anonymized then archived (reporter identity removed, target identity hashed) |
| **Moderation actions** | Permanent | Never deleted. Required for history multiplier and pattern detection |
| **Chat logs (general)** | Per [[Chat & Communication System#Chat History Retention]] (30-90 days by channel) | Purged on rolling basis |
| **Chat logs (reported messages)** | 1 year | Retained for moderation and appeal purposes, then purged |
| **Combat logs (reported matches)** | 1 year | Retained for investigation, then purged |
| **Economic transaction logs** | 1 year | Anonymized then archived |
| **Automated detection flags** | 6 months | Purged (flags that led to action are retained via the action record) |
| **Appeal records** | Permanent | Retained alongside the action record |
| **Moderator action logs** | Permanent | Required for moderator accountability audits |

### Deleted Accounts

When a player deletes their account:

| Data Type | Handling |
|-----------|----------|
| **Reports filed by the player** | Reporter identity anonymized ("deleted-account-XXXX"). Report content and outcome retained |
| **Reports received by the player** | Target identity anonymized. Report content and outcome retained |
| **Moderation actions against the player** | Player identity anonymized but action records retained (required for pattern detection on alt accounts via device/IP matching) |
| **Chat logs** | Player's messages anonymized in retained logs (replaced with "[deleted user]") |
| **Combat logs** | Player identity anonymized in retained logs |
| **Economic history** | Anonymized. Transaction records retained for economy integrity monitoring |

Anonymization uses irreversible one-way hashing. The original player identity cannot be recovered from anonymized records.

---

## MVP Scope

### MVP (Launch)

- Report submission flow (all categories, basic evidence capture)
- Investigation queue with priority scoring
- Basic moderator tools: mute, kick, temp ban, player history view
- Standard escalation path (Warning through Permanent Ban)
- Chat toxicity detection (Tier 1-3, per [[Chat & Communication System]])
- Spam detection and rate limiting (per [[Chat & Communication System]])
- Speed hacking detection (server-side timing validation)
- Web-based appeal form with 72-hour SLA
- Data retention policy enforced

### Post-MVP Phase 1

- Botting detection (combat pattern analysis)
- Economy anomaly detection
- Watch List functionality
- Trusted Reporter system
- In-game appeal form
- Economic rollback tool
- Moderator accountability dashboard

### Post-MVP Phase 2

- Real-time spectate tool for moderators (requires [[Spectator Mode]] Pass 1)
- Advanced evidence package (full behavioral identity integration)
- Social graph analysis for organized RMT/griefing detection
- Machine learning toxicity model (replaces heuristic pattern matching)
- Community moderation features (volunteer moderator program, if warranted by community size)

---

## Related Pages

- [[Chat & Communication System]] — Player-facing moderation tools, profanity filter, report flow, chat retention
- [[Combat System]] — Combat integrity, phase timing, server-authoritative state
- [[Combat Logs System]] — Evidence source for cheating and griefing reports
- [[PvP Structure]] — Anti-grief systems, ranked integrity
- [[Behavioral Identity]] — Player behavior data used in evidence packages
- [[Account & Security System]] — Account-level security, device fingerprinting, IP tracking
- [[Spectator Mode]] — Moderator spectate tool dependency
- [[Economy Model]] — Economic integrity, transaction monitoring
- [[Notification & Mail System]] — System mail delivery for moderator communications
- [[MVP Design]] — Feature phasing
