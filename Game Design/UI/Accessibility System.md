---
tags:
  - gdd
  - ui
  - accessibility
---

# Accessibility System

A text-based game has no excuse to be inaccessible. Every system in Stormbound Online must have a non-visual, non-color-dependent, non-time-pressured path to full functionality. Accessibility is not an accommodation — it is a design constraint applied from the foundation.

---

## Design Philosophy

### Core Principles

1. **Text-First is Accessibility-First** — a reactive text-based MMORPG is inherently closer to screen readers, keyboard navigation, and cognitive clarity than any graphical game. We do not squander that advantage.
2. **No Information Locked Behind a Single Channel** — color, sound, timing, and spatial arrangement are all redundant. Every piece of game state is available through at least two independent channels.
3. **Competitive Integrity with Accommodation** — accessibility settings never create unfair advantage in ranked play. Where timer extensions are needed, matchmaking handles parity.
4. **Progressive Disclosure Serves Everyone** — the same information density controls that help cognitively disabled players also help new players and returning players. Good accessibility is good design.

### Compliance Targets

| Standard | Target | Notes |
|----------|--------|-------|
| WCAG 2.2 | AA minimum, AAA where feasible | Text contrast, keyboard access, timing adjustments |
| CVAA | Full compliance | Communication features accessible |
| Game Accessibility Guidelines | Advanced tier | gameaccessibilityguidelines.com |

---

## Visual Accessibility

### Colorblind Modes

The game uses 9 element colors (see [[UI Design#Element Colors]]) and 8 semantic colors. All must remain distinguishable under three colorblind simulation types.

#### Element Color Palettes

| Element | Default | Deuteranopia | Protanopia | Tritanopia |
|---------|---------|-------------|------------|------------|
| Fire | `#FF4500` Orange-Red | `#E8A100` Amber | `#D4A800` Gold | `#FF4500` Orange-Red |
| Storm | `#00BFFF` Electric Blue | `#0080FF` Strong Blue | `#0080FF` Strong Blue | `#00E5CC` Teal |
| Nature | `#32CD32` Green | `#FFD300` Yellow | `#FFD300` Yellow | `#32CD32` Green |
| Light | `#FFD700` White-Gold | `#FFD700` White-Gold | `#FFD700` White-Gold | `#FFB3D9` Pink-Gold |
| Void | `#6A0DAD` Deep Purple | `#0040C0` Deep Blue | `#0050D0` Royal Blue | `#6A0DAD` Deep Purple |
| Water | `#00CED1` Cyan | `#00A0E0` Sky Blue | `#0090D0` Med Blue | `#00CED1` Cyan |
| Wind | `#C0C0C0` Silver | `#C0C0C0` Silver | `#C0C0C0` Silver | `#C0C0C0` Silver |
| Physical | `#708090` Steel Gray | `#708090` Steel Gray | `#708090` Steel Gray | `#708090` Steel Gray |
| Earth | `#FFBF00` Brown-Amber | `#CC6600` Burnt Orange | `#B85C00` Deep Orange | `#FFBF00` Brown-Amber |

**Design rule:** Adjacent element colors within any palette must maintain a minimum contrast ratio of 3:1 against each other and 4.5:1 against the background.

#### Semantic Color Palettes

| Meaning | Default | Deuteranopia | Protanopia | Tritanopia |
|---------|---------|-------------|------------|------------|
| Damage dealt | Red `#FF3333` | Orange `#FF8800` | Orange `#FF8800` | Red `#FF3333` |
| Healing | Green `#33CC33` | Blue `#3399FF` | Blue `#3399FF` | Green `#33CC33` |
| Buff gained | Cyan `#00CED1` | Cyan `#00CED1` | Cyan `#00CED1` | Teal `#00B3A0` |
| Debuff applied | Orange `#FFA500` | Yellow `#FFD000` | Yellow `#FFD000` | Pink `#FF80AA` |
| Critical hit | Gold + bold `#FFD700` | Gold + bold `#FFD700` | Gold + bold `#FFD700` | Gold + bold `#FFD700` |
| System message | Gray `#808080` | Gray `#808080` | Gray `#808080` | Gray `#808080` |
| Player name | White `#FFFFFF` | White `#FFFFFF` | White `#FFFFFF` | White `#FFFFFF` |
| NPC dialogue | Light purple `#CC99FF` | Light blue `#99CCFF` | Light blue `#99CCFF` | Light purple `#CC99FF` |

#### Symbol Redundancy

Every color-coded element also carries a unique text symbol that appears alongside the color. Color is never the sole differentiator.

| Element | Symbol | Example in Log |
|---------|--------|---------------|
| Fire | `(F)` | `Storm Strike (F) 187 damage` |
| Storm | `(S)` | `Storm Strike (S) 187 damage` |
| Nature | `(N)` | `Entangle (N) root applied` |
| Light | `(L)` | `Smite (L) 142 damage` |
| Void | `(V)` | `Consume (V) absorb 12` |
| Water | `(W)` | `Tidal Surge (W) 98 damage` |
| Wind | `(Wi)` | `Gust (Wi) displacement` |
| Physical | `(P)` | `Force Strike (P) 112 damage` |
| Earth | `(E)` | `Tremor (E) stagger applied` |

| Semantic | Symbol |
|----------|--------|
| Damage dealt | `[-]` |
| Healing | `[+]` |
| Buff gained | `[^]` |
| Debuff applied | `[v]` |
| Critical hit | `[!]` |

**Setting:** `Display > Color Mode` — Default / Deuteranopia / Protanopia / Tritanopia / Custom
**Setting:** `Display > Symbol Tags` — Off / Semantic Only / Element Only / All

### High Contrast Mode

Replaces the standard dark UI theme with maximum contrast rendering.

| Property | Standard | High Contrast |
|----------|----------|---------------|
| Background | Dark gray `#1A1A2E` | Pure black `#000000` |
| Primary text | Light gray `#D0D0D0` | Pure white `#FFFFFF` |
| Panel borders | Subtle `#333344` | Bright white `#FFFFFF` |
| Active element highlight | Accent color | Yellow outline `#FFFF00` + 3px border |
| Focus indicator | 2px outline | 4px high-contrast yellow outline |
| Combat log separators | ASCII lines | Double-weight ASCII + blank line padding |

**Setting:** `Display > High Contrast` — On / Off

### Font Options

| Setting | Options | Default |
|---------|---------|---------|
| Font size | Small (12px) / Medium (16px) / Large (20px) / Extra-Large (28px) | Medium |
| Font family | System Default / Monospace / Sans-Serif / Dyslexia-Friendly (OpenDyslexic) | System Default |
| Line spacing | Compact (1.2) / Standard (1.5) / Relaxed (1.8) / Wide (2.2) | Standard |
| Letter spacing | Tight / Normal / Wide / Extra-Wide | Normal |

**Constraint:** All UI panels must reflow correctly at Extra-Large font size. No text truncation. Scroll containers expand as needed.

**Setting:** `Display > Font Size`, `Display > Font Family`, `Display > Line Spacing`, `Display > Letter Spacing`

---

## Combat Timer Accessibility

The [[Combat System#Phase 2 Commit 8-10 seconds player input|Commit phase]] is the only timed player input in combat. All other phases are automatic. Timer accessibility is critical.

### Timer Modes

| Mode | Commit Duration | Availability |
|------|----------------|-------------|
| **Standard** | 8-10 seconds | All modes |
| **Extended** | 15-20 seconds | PvE always. PvP casual/unranked only. |
| **Relaxed** | 30 seconds | PvE always. PvP casual/unranked only. |
| **Untimed** | No limit (confirm when ready) | Solo PvE only. |

**Ranked PvP rule:** Standard timer only. This preserves competitive integrity. Players who require Extended or Relaxed timers compete in unranked modes with full reward parity (no progression penalty).

### Timer Visual Indicators

| Time Remaining | Indicator |
|----------------|-----------|
| >50% | Steady progress bar, neutral color |
| 25-50% | Progress bar pulses slowly, amber color |
| <25% | Progress bar pulses rapidly, red color + text countdown in seconds |
| <5 seconds | Large numeric countdown overlays the commit panel |

**Accessibility additions:**
- Optional audible tick at 5-second, 3-second, and 1-second marks
- Screen reader announces "5 seconds remaining," "3 seconds," "1 second"
- Optional vibration/haptic feedback (mobile/controller) at thresholds

### Auto-Commit Preferences

When the timer expires, the system auto-commits. Players configure behavior in advance.

| Setting | Behavior |
|---------|----------|
| **Auto-commit: In Order** | Commits drawn actions in draw order (default) |
| **Auto-commit: Priority** | Player pre-ranks action types by preference. System selects top-priority available actions. |
| **Auto-commit: Repeat Last** | Repeats the previous round's commit if valid actions are available |
| **Auto-commit: Defensive** | Always selects the most defensive available actions |

**Setting:** `Combat > Timer Mode`, `Combat > Auto-Commit Behavior`

---

## Screen Reader Support

### Requirements

- All UI panels expose a complete accessible DOM / ARIA tree
- Every interactive element has a descriptive `aria-label`
- Every panel has an `aria-live` region for dynamic updates
- Focus management follows logical reading order, not visual layout

### Panel Announcements

| Panel | Screen Reader Behavior |
|-------|----------------------|
| **Draw Phase** | Announces: "Round [N]. Draw phase. [X] actions available." Then reads each action with domain and type. |
| **Commit Phase** | Announces: "Commit phase. [X] seconds. [Y] slots to fill." Confirms each slot assignment. |
| **Clash Phase** | Reads each slot resolution sequentially. Announces damage, effects, and state changes. |
| **Aftermath Phase** | Announces mutations, momentum change, echoes, reveals. |
| **Enemy Intent** | Announces: "Enemy [name]. Intent: [category]. Domain: [domain]. Hint: [text]." |
| **Round Result** | Announces: "Round result. You dealt [X], took [Y]. Your Integrity [A]. Opponent Integrity [B]." |

### Combat Log as Structured Text

The combat log (see [[Combat Logs System]]) supports two reading modes:

**Narrative Mode (default):**
Reads the log as written, including flavor text and narrative signals. Preserves the information-warfare layer for screen reader users.

**Mechanical Mode:**
Strips all flavor text. Reads pure data:

```
Round 7. Slot 1. You: Storm Strike, Tempest, 187 damage to opponent.
Chain stack increased to 4. Opponent displaced 1 position.
Opponent: Fortify, 50% reduction. Damage reduced to 94.
Counter-Energy stored: 94.
```

**Setting:** `Accessibility > Screen Reader > Log Mode` — Narrative / Mechanical

### Phase Transition Announcements

Every phase transition is announced via `aria-live="assertive"`:

- "Draw phase begins."
- "Commit phase begins. [X] seconds."
- "Commits locked. Clash resolving."
- "Aftermath. Round [N] complete."

### Interactive Element Labels

| Element | ARIA Label Pattern |
|---------|-------------------|
| Action button in Draw | "[Action Name], [Domain], [Type: Offensive/Defensive/Utility]" |
| Commit slot | "Commit slot [N] of [max]. Currently: [empty/action name]" |
| Conditional slot toggle | "Slot 3 conditional. If opponent attacks: [action]. Otherwise: [action]." |
| Overcharge button | "Overcharge [gem name]. Plus 30% power. Minus 15 stability. Currently [stability]." |
| Lock-in button | "Lock in commit. [N] actions queued." |
| Momentum display | "Momentum [value] of 10. Zone: [Desperation/Pressure/Neutral/Advantage/Dominance]." |
| Stability display | "Gem stability. [Domain]: [value], [state: Stable/Shifting/Unstable/Critical]." |

---

## Motor Accessibility

### Full Keyboard Navigation

Every interaction in the game is reachable via keyboard. No mouse-only actions exist.

#### Global Hotkeys

| Key | Action |
|-----|--------|
| `Tab` | Next interactive element |
| `Shift+Tab` | Previous interactive element |
| `Enter` / `Space` | Activate focused element |
| `Escape` | Close current panel / cancel action |
| `F1` | Help overlay for current context |
| `F5` | Open accessibility settings (quick access) |
| `` ` `` (backtick) | Open chat |

#### Combat Hotkeys

| Key | Action |
|-----|--------|
| `1-5` | Select drawn action by position |
| `Q, W, E` | Assign to commit slot 1, 2, 3 |
| `R` | Toggle conditional mode on slot 3 |
| `O` | Overcharge selected gem |
| `Enter` | Lock in commit |
| `Z` | Undo last slot assignment |
| `X` | Clear all slots |
| `C` | Cycle combat log filter (All / Damage / Mutations / Resonance) |
| `L` | Jump focus to combat log |
| `I` | Jump focus to enemy intent panel |

#### Tab Order (Combat)

1. Action bar (drawn actions, left to right)
2. Commit slots (slot 1, slot 2, slot 3)
3. Overcharge button
4. Lock-in button
5. Combat log (scrollable)
6. Status bar (Integrity, Momentum, Stability)
7. Enemy intent panel

### Sticky Keys

Full sticky keys support. Modifier keys (Shift, Ctrl, Alt) can be toggled rather than held.

**Setting:** `Accessibility > Motor > Sticky Keys` — On / Off

### Adjustable Interaction Timing

| Setting | Options | Default |
|---------|---------|---------|
| Click hold duration | 100ms / 300ms / 500ms / 1000ms | 300ms |
| Double-click window | 200ms / 400ms / 600ms / 1000ms | 400ms |
| Key repeat delay | 250ms / 500ms / 750ms / 1000ms | 500ms |
| Key repeat rate | Fast / Medium / Slow / Off | Medium |

**Setting:** `Accessibility > Motor > Interaction Timing`

### One-Handed Play Mode

Remaps all combat inputs to a single-hand layout. Two presets available:

**Left-Hand Mode:**
| Key | Action |
|-----|--------|
| `1-5` | Select drawn action |
| `Q, W, E` | Assign to commit slot 1, 2, 3 |
| `R` | Conditional toggle |
| `Tab` | Lock in commit |
| `Caps Lock` | Undo |
| `Shift` | Overcharge |

**Right-Hand Mode:**
| Key | Action |
|-----|--------|
| `6-0` | Select drawn action |
| `I, O, P` | Assign to commit slot 1, 2, 3 |
| `[` | Conditional toggle |
| `Enter` | Lock in commit |
| `Backspace` | Undo |
| `\` | Overcharge |

**Custom Rebinding:** All hotkeys are fully rebindable. No hardcoded bindings.

**Setting:** `Accessibility > Motor > One-Handed Mode` — Off / Left / Right / Custom

---

## Cognitive Accessibility

### Information Density Settings

Three density levels control how much data is shown across all panels simultaneously.

| Level | What Is Shown | What Is Hidden |
|-------|---------------|----------------|
| **Minimal** | Action names, damage totals, round results, Integrity bars. Commit slots with action names only. | Stability numbers, chain stacks, momentum sub-events, resonance proximity signals, mutation flavor text, sub-line effects (push, absorb). |
| **Standard** | Everything in Minimal + stability per gem, momentum value, resonance signals, Counter Prompts, sub-line effects. | Mutation flavor text at Momentum <7 (matches default PvP behavior). |
| **Detailed** | Full combat log as specified in [[Combat Logs System]]. All narrative signals, all mechanical sub-data. | Nothing hidden. |

**Constraint:** Information density never removes mechanically relevant data in competitive PvP. Minimal mode summarizes but does not omit — players can expand any line to see full detail.

**Setting:** `Display > Information Density` — Minimal / Standard / Detailed

### Combat Log Verbosity

Independent of information density. Controls the combat log specifically.

| Mode | Behavior |
|------|----------|
| **Full Narrative** | Default. Includes flavor text, narrative signals, resonance hints. |
| **Mechanical + Summary** | Strips flavor text. Shows pure data: action, domain, damage, effects. Round summary. |
| **Mechanical Only** | Pure data. No round summary. One line per event. Maximum scan speed. |
| **Summary Only** | Shows only round results: "R7: You dealt 299, took 94. Integrity: 78 vs 71." |

Example — Mechanical Only:
```
R7 S1: You > Storm Strike (Tempest) > Opp [-187]
R7 S1: Opp > Fortify (Aegis) [50% reduction] [-94 actual]
R7 S2: You > Corrode (Entropy) > Opp [-112] [v DEF -15% 3r]
R7 S2: Opp > Reflect (Aegis) > You [-94 CE]
R7 S3: You > Consume (Void) > Opp [absorb +12]
R7 S3: Opp > Construct (Synthesis) [zone 3r]
R7 RESULT: You [-94] Opp [-299] | Integrity 78/100 vs 71/100
```

**Setting:** `Combat > Log Verbosity` — Full Narrative / Mechanical + Summary / Mechanical Only / Summary Only

### Simplified UI Mode

Hides advanced systems behind explicit expansion. Aimed at new players and players who prefer reduced cognitive load.

| Hidden by Default | Revealed When |
|-------------------|---------------|
| Gem stability numbers | Player taps/clicks the gem icon |
| Mutation state details | Player opens gem detail panel |
| Resonance proximity hints | Player enables in settings or reaches Level 10 |
| Conditional slot 3 | Player enables in settings or completes tutorial stage 4 |
| Overcharge option | Player enables in settings or reaches Level 15 |
| Counter Prompts | Player enables in settings or reaches Level 20 |
| Momentum sub-events | Player taps the momentum bar |

**Setting:** `Display > Simplified UI` — On / Off (Off shows everything)

### Tutorial Replay

All tutorial segments are replayable from a dedicated menu. Tutorials cover:

1. Basic combat flow (Draw, Commit, Clash, Aftermath)
2. Reading the combat log
3. Gem stability and mutation
4. Momentum system
5. Resonance discovery
6. Conditional actions
7. Overcharge risk/reward
8. Group PvE coordination
9. Build publishing

**Setting:** `Help > Replay Tutorial > [Select segment]`

---

## Audio Accessibility

### Visual Indicators for All Audio Cues

No game-relevant information is delivered through audio alone. Every audio cue has a visual counterpart.

| Audio Cue | Visual Equivalent |
|-----------|-------------------|
| Phase transition chime | Phase banner text + panel border flash |
| Timer warning ticks | Progress bar pulse + numeric countdown |
| Critical hit impact | `[!]` symbol + bold + gold text |
| Resonance trigger | `RESONANCE:` tag + double-border highlight |
| Mutation threshold crossed | Stability number color change + state label update |
| Chat message received | Notification dot on chat panel + unread counter |
| Party member action | Party member name highlighted in commit log |
| Boss mechanic alert | `!! MECHANIC:` tag + panel-wide highlight |

### Closed Captions

All narrative audio (NPC voice lines, cinematic narration, boss dialogue) is displayed as closed captions in a dedicated subtitle panel.

| Setting | Options |
|---------|---------|
| Captions | On / Off |
| Caption size | Small / Medium / Large |
| Caption background | Transparent / Semi-transparent / Opaque |
| Speaker labels | On / Off (identifies who is speaking) |

### Volume Controls

Separate sliders per audio channel. All default to 80%.

| Channel | Controls |
|---------|----------|
| Master | Overall volume |
| Music | Background music, zone ambient |
| SFX | Combat sound effects, UI clicks |
| UI | Button feedback, panel transitions |
| Notifications | Chat alerts, dungeon ready, marketplace sales |
| Voice | NPC dialogue, narrator, boss callouts |

**Setting:** `Audio > Volume > [Channel]`

### Mono Audio

Collapses stereo/surround to mono output. No spatial audio information is gameplay-critical, but this ensures no information loss for single-ear headphone users or hearing-impaired players.

**Setting:** `Audio > Mono Audio` — On / Off

---

## Communication Accessibility

### Text-to-Speech for Chat

All incoming chat messages can be read aloud via system TTS engine.

| Setting | Options |
|---------|---------|
| TTS for chat | Off / Party Only / All Channels |
| TTS voice | System default / selectable voices |
| TTS speed | Slow / Normal / Fast |
| TTS volume | Separate slider (under Voice channel) |

### Speech-to-Text for Chat

Players can dictate chat messages using system speech recognition. Dictated text appears in the chat input field for review before sending.

**Setting:** `Accessibility > Communication > Speech-to-Text` — On / Off

### Pre-Built Communication Shortcuts

Quick-access communication that requires no typing. Available in party and group content.

#### Ping Wheel (Party Play)

| Ping | Message Sent | Symbol |
|------|-------------|--------|
| Attack | "Focus this target" | Crosshair |
| Defend | "Guard/protect here" | Shield |
| Wait | "Hold, not yet" | Open hand |
| Go | "Engage now" | Arrow forward |
| Help | "Need assistance" | Exclamation |
| Danger | "Incoming threat" | Warning triangle |
| Good | "Nice play" | Checkmark |
| Ready | "Ready to proceed" | Thumbs up |

**Access:** `V` key opens ping wheel. Arrow keys or number keys select. Mouse optional.

#### Quick Chat Phrases

Context-sensitive preset messages. Selectable from a radial menu or hotkeys.

**Combat context:**
- "Targeting [enemy name]"
- "Switching to defense"
- "Overcharging this round"
- "Need healing/support"
- "Watch for resonance"
- "Boss mechanic incoming"

**Hub context:**
- "Looking for party"
- "Trading — open to offers"
- "Good fight"
- "Thanks"
- "Need a moment"

**Setting:** `Communication > Quick Chat` — customizable phrase list (max 12 combat, max 8 hub)

### Symbol-Based Quick Communication

For players who prefer non-text communication, a symbol grid is available. Combines icons to form messages:

`[Target icon] + [Skull icon]` = "Kill target"
`[Shield icon] + [Arrow up]` = "Buff up"
`[Clock icon] + [Stop icon]` = "Wait"

The recipient sees both the symbols and an auto-generated text translation.

---

## Settings and Profiles

### Accessibility Profile System

All accessibility settings are bundled into saveable profiles.

| Feature | Detail |
|---------|--------|
| Profile slots | Up to 5 named profiles |
| Default profile | "Standard" — all defaults, no accessibility modifications |
| Profile switching | Instant. No reload required. Available from any screen. |
| Profile export/import | Copy profile as text string (similar to build codes) |

### Cross-Device Sync

Accessibility profiles are stored server-side and sync automatically across all devices linked to the account.

| Behavior | Detail |
|----------|--------|
| Sync trigger | On login, profiles are pulled from server |
| Conflict resolution | Most recently modified profile wins |
| Offline fallback | Local cache of last-synced profile used when offline |
| Bandwidth | Profile data is <1KB. Negligible sync cost. |

### Pre-Login Access

Accessibility settings are available from the **login screen** before the player enters the game. This is non-negotiable.

| Available at Login | Not Available at Login |
|--------------------|----------------------|
| Font size, font family, line spacing | Game-specific settings (combat timers, log verbosity) |
| High contrast mode | Profile loading (requires authentication) |
| Colorblind mode | Quick chat customization |
| Screen reader toggle | Information density |
| Audio volume controls | |
| Mono audio | |
| TTS for login UI | |

After authentication, full profile loads and overrides login-screen settings.

### Reset to Defaults

Every settings category has an individual "Reset to Default" button. A global "Reset All Accessibility Settings" option exists with a confirmation dialog.

**Setting:** `Accessibility > Reset > [Category] / All`

---

## Implementation Priority

Mapped to [[MVP Design]] development priorities.

| Priority | Feature | Rationale |
|----------|---------|-----------|
| **P0 — MVP Required** | Keyboard navigation, screen reader support, font scaling, colorblind modes, timer modes, pre-login settings | Cannot ship without. Text-based game with no keyboard support is broken, not inaccessible. |
| **P0 — MVP Required** | Symbol redundancy for colors, mechanical combat log mode | Core information channels. |
| **P1 — MVP Target** | High contrast mode, one-handed play, information density, log verbosity settings | Strong accessibility baseline. |
| **P1 — MVP Target** | Quick chat pings, auto-commit preferences, TTS for chat | Communication and motor support. |
| **P2 — Post-Launch** | Full profile system with cross-device sync, speech-to-text, symbol-based communication | Infrastructure-heavy. |
| **P2 — Post-Launch** | Custom colorblind palette editor, tutorial replay, closed captions | Polish and completeness. |

---

## Related Pages

- [[UI Design]] — Color language, panel layout, text formatting
- [[Combat System]] — Phase structure, Commit timer, round flow
- [[Combat Logs System]] — Log structure, narrative signals, information tiers
- [[MVP Design]] — Scope and development priorities
- [[GDD Hub]] — Master document index
