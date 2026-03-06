---
tags:
  - gdd
  - content
  - pvp
  - social
  - post-mvp
---

# Spectator Mode

## Design Philosophy

Combat in Stormbound Online is a conversation of prediction, commitment, and adaptation. For the two participants, this conversation is deliberately obscured — information asymmetry is a core mechanic (see [[Combat System#Information Warfare]]). Spectating inverts this. The spectator sees **more** than either player does, transforming an opaque duel into a readable, teachable, castable experience.

Spectating should make combat **more understandable**, not less. A new player watching a high-level match should be able to follow the tactical flow — who has momentum, which gems are mutating, why a conditional action was brilliant — even if they cannot yet execute at that level themselves.

Three audiences, three goals:

| Audience | Goal | Spectator Mode Delivers |
|----------|------|------------------------|
| **Learners** | Understand combat depth | Full information display reveals what both players see and decide |
| **Competitors** | Study opponents and strategies | Replay system with rewind, slow-motion, and pattern analysis |
| **Community** | Watch esports and share moments | Casting tools, tournament integration, highlight voting |

Spectating is not a passive feature bolted onto PvP. It is the bridge between playing and understanding — and between understanding and community.

---

## Observer UI

The spectator view presents a **side-by-side dual-perspective layout** showing full information for both players simultaneously. This is information neither player has access to during the match.

### Primary Display

```
+---------------------------+---+---------------------------+
|       PLAYER A            | V |       PLAYER B            |
|                           | S |                           |
|  Integrity: 78/100        |   |  Integrity: 71/100        |
|  Momentum:  7/10          |   |  Momentum:  4/10          |
|  ▓▓▓▓▓▓▓░░░ (Advantage)   |   |  ▓▓▓▓░░░░░░ (Neutral)     |
|                           |   |                           |
|  GEMS:                    |   |  GEMS:                    |
|  Kinetics  [Stable  92]   |   |  Aegis     [Shifting 68]  |
|  Tempest   [Shifting 82]  |   |  Synthesis [Stable   91]  |
|  Entropy   [Stable  87]   |   |  Kinetics  [Stable   85]  |
|  Void      [Stable  93]   |   |  Resonance [Stable   77]  |
|                           |   |                           |
|  DRAW:                    |   |  DRAW:                    |
|  [1] Force Strike (Kin)   |   |  [1] Fortify (Aegis)      |
|  [2] Storm Strike (Tmp)   |   |  [2] Reflect (Aegis)      |
|  [3] Corrode (Ent)        |   |  [3] Construct (Syn)      |
|  [4] Consume (Void)       |   |  [4] Displace (Kin)       |
|  [5] Chain Surge (Tmp)    |   |                           |
|                           |   |                           |
|  COMMIT:                  |   |  COMMIT:                  |
|  [Revealed after lock-in] |   |  [Revealed after lock-in] |
+---------------------------+---+---------------------------+
|              SHARED COMBAT LOG (CLASH)                    |
|              Momentum Graph | Damage Tracker              |
+-----------------------------------------------------------+
```

### Information Panels

| Panel | Contents | Update Frequency |
|-------|----------|-----------------|
| **Player Cards** | Name, faction badge, alignment, archetype name, season rank, win/loss record | Static per match |
| **Draw Panel** | Both players' full draws, including mutation variants and momentum/desperation tags | Per round (Phase 1) |
| **Commit Panel** | Both players' committed sequences, including conditional logic | Per round (Phase 2, revealed on lock-in) |
| **Stability Monitor** | All 8 gems (4 per player), exact stability values, mutation state labels, mutation flavor text | Per round (Phase 4) |
| **Momentum Graph** | Line graph tracking both players' momentum over all rounds | Per round |
| **Damage Tracker** | Cumulative damage dealt/received per player, damage-per-round average, domain breakdown | Per round |
| **Resonance Tracker** | Proximity indicators for both players' potential resonances, triggered resonances log | Per round |
| **Gem Domain Map** | Visual showing all revealed domains for both players, with stability color coding (green/yellow/orange/red) | Per round |

### Spectator Combat Log

The spectator combat log is an **enhanced version** of the standard combat log (see [[Combat Logs System]]). It includes:

- Both players' full Clash narration (identical to the standard log)
- Stability change annotations inline (e.g., "Tempest gem: 85 -> 82")
- Mutation flavor text for **both** players (not momentum-gated for spectators)
- Resonance proximity for **both** players (explicit countdowns, not signal language)
- Conditional action logic revealed: "Player A predicted attack -> Consume triggered (CORRECT)"
- Pattern detection annotations: "Player B has used Fortify opener 5/7 rounds"

---

## Anti-Ghosting

Spectator mode must not compromise competitive integrity. The primary defense is a **configurable broadcast delay** that ensures spectators see past game state, not current state.

### Delay Configuration

| Context | Delay | Rationale |
|---------|-------|-----------|
| **Replay** | 0 seconds | Match is already complete. No integrity risk |
| **Casual spectating** (unranked) | 15 seconds | Low stakes. Minor delay for reasonable experience |
| **Ranked spectating** | 30 seconds | Competitive integrity. ~2 rounds of delay at typical pacing |
| **Tournament spectating** | 60 seconds | Maximum protection. Tournament organizer can configure 30-60s |
| **Custom/private match** | Configurable (0-60s) | Match creator sets delay. Both players must agree to 0s |

### Enforcement

- During the delay window, the spectator sees the game state as it existed N seconds ago. The spectator UI does not indicate that a delay is active (no countdown timer showing "real" vs "delayed" state)
- **Communication block:** Spectators cannot send whispers, party chat, or any direct message to either participant during a live match. This restriction lifts when the match ends
- **Social channel restriction:** Spectators can use spectator chat (see [[#Spectator Capacity]]) and global/region chat, but any message containing participant names or match-specific terminology (gem names, action names, domain references) during an active spectated match is **held for the duration of the delay** before delivery to public channels
- **Friend list suppression:** If a spectator is on a participant's friend list, the participant's client does not display the spectator's online status changes or activity status during the match
- **API restriction:** No real-time match state exposed via any external API during live matches. Post-match data available after completion

### Edge Cases

| Scenario | Handling |
|----------|----------|
| Spectator disconnects and reconnects | Resumes from current delayed state, not live state |
| Match ends before delay catches up | Delay continues until spectator sees the final round naturally. Result is not spoiled |
| Player streams their own match | Player's own stream is their responsibility. Spectator mode delay still applies to all spectator clients |

---

## Casting Tools

Designed for content creators, tournament commentators, and community figures who narrate matches for an audience.

### Annotation System

Casters can overlay visual annotations on the spectator view in real-time:

| Tool | Function | Shortcut |
|------|----------|----------|
| **Circle** | Draw a circle around a UI element (gem, action, stat) to draw attention | `C + click` |
| **Arrow** | Draw an arrow between two elements to show cause-and-effect | `A + drag` |
| **Highlight Box** | Colored rectangle overlay to emphasize a region of the screen | `H + drag` |
| **Text Label** | Short text annotation (50 character max) placed on screen | `T + click` |
| **Clear All** | Remove all annotations | `Esc` |

Annotations are visible to all spectators watching the caster's feed. They persist for 8 seconds then fade, or until manually cleared.

### Replay Controls

| Control | Function |
|---------|----------|
| **Slow Motion** | 0.25x, 0.5x playback speed during Clash phase resolution |
| **Rewind** | Step back by round (full round replay) or by phase (granular) |
| **Fast Forward** | 2x, 4x speed through Draw/Commit phases (useful for replays) |
| **Pause** | Freeze on any phase for analysis |
| **Bookmark** | Mark a specific round for quick navigation ("Key moment at R7") |
| **Loop** | Repeat a specific round or phase continuously |

### Picture-in-Picture (PiP)

- Toggle between **side-by-side view** (default) and **focused view** (one player's full perspective)
- PiP window shows the other player's perspective in a smaller overlay
- Keyboard toggle: `1` for Player A focus, `2` for Player B focus, `3` for side-by-side
- During tournament multi-match casting, PiP can show a different active match

### Stat Comparison Overlay

A toggleable overlay that presents head-to-head statistics:

```
STAT COMPARISON ══════════════════════
               Player A    Player B
  Damage/Round:   142         98
  Avg Momentum:   6.8        4.2
  Gems Mutated:   2/4        1/4
  Conditionals:   3/5 (60%)  1/3 (33%)
  Resonances:     2          0
  Domain Spread:  4 domains  3 domains
══════════════════════════════════════
```

Updated in real-time. Useful for casters summarizing match flow.

---

## Tournament Integration

Spectator mode integrates with the [[PvP Structure#PvP Seasons|seasonal championship system]] and the [[Factions System#Season Championships|Arena League quarterly tournaments]].

### Bracket Display

- Full tournament bracket visible from the spectator lobby
- Bracket updates in real-time as matches conclude
- Each bracket node shows: player names, archetype names, match score (e.g., 2-1), link to replay
- Completed matches display the winner highlighted; in-progress matches pulse with an activity indicator
- Losers bracket supported for double-elimination formats

### Match Schedule

- Upcoming matches listed with estimated start times
- "Remind Me" button queues a notification when a specific match goes live
- Current active matches listed with spectator count and round number

### Automatic Camera Switching

For tournament broadcasts with multiple simultaneous matches:

| Mode | Behavior |
|------|----------|
| **Manual** | Caster selects which match to display |
| **Auto-Priority** | System switches to the match with the highest drama score (close Integrity, high momentum swings, resonance triggers) |
| **Round-Robin** | Cycles through active matches on a timer (configurable interval: 30s-120s) |
| **Elimination Focus** | Prioritizes matches where a player is one round from elimination |

Drama score is calculated from: Integrity difference (closer = higher), momentum swing magnitude, resonance trigger count, round count (longer matches score higher), and player rank (higher-ranked players score higher).

### Player Profiles

Accessible from the bracket display or spectator lobby:

| Field | Source |
|-------|--------|
| Player name, faction badge, alignment | Account data |
| Season rank and rating | [[PvP Structure]] ranked ladder |
| Win/loss record (season and all-time) | Match history |
| Signature build (most-used archetype) | Loadout tracking |
| Gem domain preferences (pie chart) | Match history analysis |
| Notable achievements | Tournament placements, titles |
| Recent match history (last 10) | With replay links |

---

## Spectator Capacity

### Limits

| Context | Max Spectators | Rationale |
|---------|---------------|-----------|
| **Casual match** | 20 | Low server overhead for non-featured matches |
| **Ranked match** | 50 | Moderate interest, manageable load |
| **Tournament match** | 500 | High-profile matches need audience capacity |
| **Featured/Finals match** | 2,000 | Peak events, server resources pre-allocated |

When a match reaches capacity, additional spectators are placed in a queue. If a spectator leaves, the next queued spectator is admitted.

### Spectator Chat

Spectator chat is a **dedicated channel** separate from all player-facing channels (see [[Chat & Communication System]]).

| Property | Specification |
|----------|--------------|
| **Scope** | Per-match. Each spectated match has its own spectator chat instance |
| **Visibility** | Spectators only. Neither participant can see spectator chat |
| **Rate limit** | 6 messages per minute (same as Global chat) |
| **Moderation** | Standard profanity filter applies. Reports follow [[Chat & Communication System#Report Flow]] |
| **Persistence** | Session only. Chat is cleared when the match ends |
| **Character limit** | 200 characters per message |

### Spectator Reactions

Non-intrusive reaction system for spectators to express engagement without flooding chat:

| Reaction | Trigger | Display |
|----------|---------|---------|
| **Hype** | Big damage, resonance trigger, momentum swing | Aggregate counter shown as "42 Hype" near the event |
| **Predict** | Before Clash resolves, spectators can predict which player wins the round | Post-Clash: "67% predicted Player A" |
| **GG** | Match conclusion | Aggregate counter displayed on victory screen |

Reactions are **aggregated, not individual**. No single spectator's reaction is highlighted. This prevents spam and keeps the focus on the match. Participants do not see reactions during the match. Post-match, the participant receives a summary: "Watched by 127 spectators. 89 Hype reactions. 94% predicted your Round 11 comeback."

---

## Replay System

### Storage

- Full match state is recorded server-side for every PvP match (ranked and unranked)
- Replay data includes: every draw, commit, clash resolution, aftermath state change, exact stability values, momentum values, and timing data
- Storage retention: 90 days for standard matches, permanent for tournament matches
- Replay file size estimate: ~50-100 KB per match (state deltas, not video)

### Replay Codes

- Every completed match generates a unique **replay code** (format: `RPL-XXXXXX-XXXXXX`)
- Replay codes are shareable in chat, build guides, and external platforms
- Entering a replay code in the spectator panel loads the full match replay
- Replay codes are permanent for tournament matches; standard match codes expire with the replay data (90 days)

### Replay Browser

| Tab | Contents |
|-----|----------|
| **Recent** | Player's own last 50 matches (mirrors [[Combat Logs System#Combat History Archive]]) |
| **Featured** | Curated matches selected by community vote or moderator highlight |
| **Top Ranked** | Highest-rated players' recent matches, auto-populated from ranked ladder |
| **Tournament** | All tournament match replays, organized by event and bracket |
| **Search** | Search by player name, archetype, domain, date range, or replay code |

### Community Highlights

- After watching a replay, spectators can **nominate a specific round** as a highlight
- Highlights with the most nominations surface in the Featured tab
- Weekly "Top 5 Moments" curated from the most-nominated highlights
- Highlight clips are shareable as replay codes with a round bookmark (format: `RPL-XXXXXX-XXXXXX#R7`)
- Feeds into the [[Creator Ecosystem]] — build guide creators can embed replay references

### Highlight Voting

| Action | Mechanism |
|--------|-----------|
| **Nominate** | While watching a replay, click "Highlight" on any round. Adds one nomination |
| **Vote** | In the Featured tab, upvote or downvote existing highlights |
| **Weekly Reset** | Highlight scores reset weekly. Top highlights are archived permanently |
| **Abuse Prevention** | 1 nomination per replay per player. 20 votes per day cap. Account must be level 10+ |

---

## MVP Scope

Spectator mode is a **post-MVP feature** delivered in three passes.

### Pass 1: Basic Spectator (Post-Launch Update 1)

- Spectate any live match via player profile or friends list
- Configurable delay (15s casual, 30s ranked)
- Full information observer UI (side-by-side, both players' draws/commits/stability)
- Enhanced spectator combat log
- Spectator chat (per-match)
- Basic replay viewing (own matches only, via combat history archive)
- Anti-ghosting communication block
- Spectator capacity: 20 casual, 50 ranked

**Dependencies:** [[Combat Logs System]], [[Chat & Communication System]], [[PvP Structure]]

### Pass 2: Casting and Replay Tools (Post-Launch Update 2-3)

- Annotation system (circle, arrow, highlight, text)
- Replay controls (slow-motion, rewind, fast-forward, pause, bookmark, loop)
- Picture-in-picture toggle
- Stat comparison overlay
- Replay codes (shareable)
- Replay browser (Recent, Top Ranked, Search tabs)
- Spectator reactions (Hype, Predict, GG)
- Increased spectator capacity: 500 for featured matches

**Dependencies:** Pass 1 complete, [[Creator Ecosystem]]

### Pass 3: Tournament and Community (Post-Launch Update 4+)

- Tournament bracket display and match schedule
- Automatic camera switching (Manual, Auto-Priority, Round-Robin, Elimination Focus)
- Player profiles in spectator lobby
- Community highlight nominations and voting
- Featured tab and weekly Top 5 curation
- Full replay browser (all tabs including Tournament and Featured)
- Spectator capacity: 2,000 for finals
- Highlight clip sharing with round bookmarks

**Dependencies:** Pass 2 complete, [[PvP Structure#PvP Seasons]], [[Factions System#Season Championships]]

---

## Related Pages

- [[Combat System]] — Round structure, momentum, information warfare
- [[Combat Logs System]] — Log format, narrative signals, enhanced log at high momentum
- [[PvP Structure]] — Arena modes, ranked seasons, tournament structure
- [[Factions System]] — Arena League faction, season championships
- [[Chat & Communication System]] — Spectator chat integration, anti-ghosting channel rules
- [[Creator Ecosystem]] — Replay sharing, highlight embedding, build proof
- [[UI Design]] — Panel layout, color language, spectator view integration
- [[MVP Design]] — Post-MVP phasing
