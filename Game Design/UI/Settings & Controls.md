---
tags:
  - gdd
  - ui
  - settings
---

# Settings & Controls

Player-configurable settings, keybindings, and preferences for Stormbound Online. All settings are accessible via the Settings panel (default: `Esc`) and persist per-account with profile support.

---

## Keybindings

All keybindings are fully rebindable. Conflicts are flagged on assignment. Secondary bindings supported for every action.

### Combat

| Action | Default Binding | Description |
|--------|----------------|-------------|
| Commit Slot 1 | `1` | Assign drawn action to commit slot 1 |
| Commit Slot 2 | `2` | Assign drawn action to commit slot 2 |
| Commit Slot 3 | `3` | Assign drawn action to commit slot 3 |
| Conditional Action Toggle | `C` | Switch slot 3 between standard and conditional mode |
| Overcharge | `O` | Open overcharge target selection for a gem |
| Lock In Commit | `Enter` | Confirm and lock in the current commit sequence |
| Cancel / Undo Last Slot | `Backspace` | Remove the last assigned slot (before lock-in) |
| Clear All Slots | `Shift+Backspace` | Clear entire commit sequence |
| Cycle Drawn Actions | `Tab` | Cycle focus through available drawn actions |

### Panels

| Action | Default Binding | Description |
|--------|----------------|-------------|
| Open Inventory | `I` | Toggle inventory panel |
| Open Build Panel | `B` | Toggle gem loadout and build configuration |
| Open Quest Log | `J` | Toggle quest log |
| Open Map | `M` | Toggle region map |
| Open Settings | `Esc` | Toggle settings panel |
| Open Social Panel | `P` | Toggle friends, party, and guild panel |
| Open Combat Log (History) | `L` | Toggle scrollable combat log history |

### Communication

| Action | Default Binding | Description |
|--------|----------------|-------------|
| Focus Chat Input | `/` | Move cursor to chat input field |
| Cycle Chat Channel | `Shift+Tab` | Cycle between Region, Party, Guild, Whisper channels |
| Reply to Last Whisper | `R` | Pre-fill chat with whisper to last sender |

### Quick Actions

| Action | Default Binding | Description |
|--------|----------------|-------------|
| Inspect Target | `X` | Inspect the currently selected player or enemy |
| Quick Heal / Potion | `Q` | Use the first available consumable |
| Queue Dungeon | `Shift+D` | Open dungeon queue dialog |
| Toggle HUD | `Alt+H` | Show/hide the full HUD overlay |

---

## Display Settings

| Setting | Options | Default |
|---------|---------|---------|
| Text Size | Small, Medium, Large, Extra Large | Medium |
| Font Selection | System Default, Monospace, Serif, Sans-Serif | System Default |
| Theme | Dark, Light, Custom | Dark |
| UI Panel Layout | Locked, Draggable | Locked |
| Combat Log Scroll Speed | Slow, Normal, Fast, Instant | Normal |
| Panel Opacity | 50%–100% slider | 90% |
| UI Mode | Compact, Expanded | Expanded |
| Damage Number Display | Show, Hide | Show |
| Action Bar Position | Bottom, Left, Right | Bottom |

### Theme Details

- **Dark** — dark background, high-contrast text. Optimized for extended sessions
- **Light** — light background, dark text. Accessibility option
- **Custom** — player selects background color, text color, accent color, and panel border color. Must pass a minimum contrast ratio check (WCAG AA) before saving

### UI Mode Details

- **Expanded** — all panels display full headers, descriptions, and spacing. Default for new players
- **Compact** — reduced padding, abbreviated labels, smaller status bar. Designed for experienced players who prioritize information density

---

## Combat Settings

| Setting | Options | Default |
|---------|---------|---------|
| Auto-Commit Behavior (on timeout) | Commit In Order, Random, Last Used Sequence | Commit In Order |
| Commit Timer Display | Countdown Bar, Numeric, Both | Both |
| Combat Log Detail Level | Narrative, Mechanical, Both | Both |
| Damage Number Display | Show, Hide | Show |
| Clash Narration Speed | Slow, Normal, Fast, Skip | Normal |
| Momentum Display | Numeric, Bar, Both | Both |
| Stability Display | Numeric, Bar, Both | Bar |

### Auto-Commit Behavior

When the commit timer expires without the player locking in:

- **Commit In Order** — system commits drawn actions in the order they appear (slots 1, 2, 3). Predictable. Safe
- **Random** — system commits drawn actions in random order. Unpredictable but avoids exploitable patterns
- **Last Used Sequence** — system replays the player's most recent commit sequence using available drawn actions. Falls back to Commit In Order if no match

### Combat Log Detail Level

- **Narrative** — prose-style descriptions with behavioral voice (see [[Behavioral Identity]]). Emphasizes flavor and readability
- **Mechanical** — raw numbers, status changes, and system events. No prose. Optimized for log-reading skill
- **Both** — narrative text with mechanical data inline. Highest information density

### Clash Narration Speed

Controls how quickly the Clash phase text renders during the narrated resolution:

- **Slow** — 1.5x standard duration. Full dramatic pacing
- **Normal** — standard narration timing (~3-5 seconds per clash)
- **Fast** — 0.5x duration. Rapid text render
- **Skip** — instant render. Full clash result appears immediately

---

## Privacy Settings

Controls what other players can see about you. Referenced systems: [[Behavioral Identity]], [[Cosmetic System]].

| Setting | Options | Default |
|---------|---------|---------|
| Build Card Visibility | Public, Friends Only, Private | Public |
| Online Status Visibility | Everyone, Friends Only, Invisible | Everyone |
| Inspect Permissions | Anyone, Friends & Guild, No One | Anyone |
| Chat Visibility | All Channels, Friends & Guild Only, Disabled | All Channels |
| Behavioral Identity Display | Full, Partial, Hidden | Full |
| PvP Masking Mode | Open, Veiled, Shrouded | Open |
| Whisper Permissions | Anyone, Friends Only, No One | Anyone |

### Build Card Visibility

- **Public** — any player can view your full build card (subject to section-level toggles defined in [[Behavioral Identity]])
- **Friends Only** — only friends and guild members see your build card. Others see name, alignment badge, and level only
- **Private** — no one sees your build card. You appear with name and level only

### Behavioral Identity Display

Controls which behavioral layers are visible to others (see [[Behavioral Identity]] — Five Cosmetic Layers):

- **Full** — aura, combat log voice, hit effects, behavioral title, and build card behavioral section all visible
- **Partial** — aura and behavioral title visible. Hit effects revert to plain. Build card behavioral section hidden
- **Hidden** — all behavioral cosmetic layers suppressed. Equivalent to Veiled PvP masking in all contexts

### PvP Masking Mode

See [[Behavioral Identity]] — PvP Anonymity System for full masking mode definitions. Shrouded requires the "The Unseen" milestone. Anonymous is tournament-only (admin-controlled).

---

## Notification Settings

Per-category controls for all notification types. Full notification taxonomy will be defined in [[Notification & Mail System]].

| Category | Toast | Sound | Default State |
|----------|-------|-------|---------------|
| Combat (queue ready, duel invite) | Toggle | Toggle | On |
| Social (friend request, guild invite) | Toggle | Toggle | On |
| Economy (marketplace sale, bid outbid) | Toggle | Toggle | On |
| Quest (objective complete, new quest) | Toggle | Toggle | On |
| System (maintenance, patch notes) | Toggle | Toggle | On |
| Build (comments on published build) | Toggle | Toggle | On |
| Achievement (milestone unlocked) | Toggle | Toggle | On |

### Additional Notification Settings

| Setting | Options | Default |
|---------|---------|---------|
| Toast Duration | Short (3s), Medium (5s), Long (8s) | Medium (5s) |
| Toast Position | Top-Right, Top-Left, Bottom-Right, Bottom-Left | Top-Right |
| Do Not Disturb | On, Off | Off |
| DND Scheduled Hours | Time range picker (24h) | Disabled |
| Max Concurrent Toasts | 1–5 slider | 3 |

### Do Not Disturb

When active, DND suppresses all toast notifications and notification sounds. Badge counts still accumulate. Critical system notifications (server shutdown, maintenance) bypass DND.

---

## Audio Settings

| Channel | Control | Default |
|---------|---------|---------|
| Master Volume | 0–100 slider | 80 |
| Music | 0–100 slider | 70 |
| SFX | 0–100 slider | 80 |
| UI Sounds | 0–100 slider | 60 |
| Combat Sounds | 0–100 slider | 80 |
| Ambient | 0–100 slider | 50 |
| Notifications | 0–100 slider | 70 |

### Additional Audio Settings

| Setting | Options | Default |
|---------|---------|---------|
| Mute All | Toggle | Off |
| Mute Music | Toggle | Off |
| Mute SFX | Toggle | Off |
| Mute Notifications | Toggle | Off |
| Mono Audio | On, Off | Off |
| Audio on Focus Loss | Mute, Reduce (50%), Unchanged | Reduce (50%) |

### Mono Audio

Combines stereo channels into a single mono output. Accessibility feature for single-speaker setups or hearing impairment.

---

## Gameplay Settings

| Setting | Options | Default |
|---------|---------|---------|
| Language | English (additional languages post-MVP) | English |
| Tooltip Detail Level | Brief, Detailed, Advanced | Detailed |
| Confirm Before Overcharge | On, Off | On |
| Confirm Before High-Risk Content | On, Off | On |
| Auto-Loot | All, Uncommon+, Rare+, Off | All |
| Auto-Sort Inventory | On, Off | Off |
| Combat Tutorial Hints | On, Off | On |
| Show Build Codes on Inspect | On, Off | On |

### Tooltip Detail Level

- **Brief** — name and one-line summary. Minimal screen clutter
- **Detailed** — name, description, stats, cooldown, resource cost. Standard play experience
- **Advanced** — all of Detailed plus: damage formula factors, scaling coefficients, resonance compatibility notes, mutation tendency. Designed for theorycrafters

### Auto-Loot

Controls what rarity tiers are automatically collected at end of combat:

- **All** — everything goes to inventory
- **Uncommon+** — Common items are skipped (can be picked up manually from the loot panel)
- **Rare+** — Common and Uncommon items skipped
- **Off** — all loot requires manual pickup

---

## Account Settings

References: [[Account & Security System]].

| Setting | Description |
|---------|-------------|
| Change Password | Requires current password + new password confirmation |
| Change Email | Requires current password + email verification on both old and new addresses |
| Two-Factor Authentication | Enable/disable 2FA via authenticator app or SMS. Recovery codes generated on enable |
| Device Management | View active sessions by device/IP. Revoke individual sessions or all other sessions |
| Data Export | Request full account data export (GDPR compliant). Processing time: up to 48 hours. Delivered as downloadable archive |
| Account Deletion | Permanent. Requires password + 2FA confirmation + 30-day cooling period with cancellation option |
| Login History | View recent login timestamps, IP addresses, and device types |

### Account Deletion Process

1. Player initiates deletion request
2. Confirm via password + 2FA (if enabled)
3. Account enters 30-day grace period — login is still possible, all data intact
4. Player can cancel deletion at any point during grace period
5. After 30 days, account data is permanently and irreversibly deleted
6. Published builds are anonymized (author removed, build data preserved for community)

---

## Settings Profiles

Players can save, load, and manage multiple settings configurations.

| Feature | Description |
|---------|-------------|
| Save Profile | Save all current settings as a named profile. Max 5 profiles per account |
| Load Profile | Apply a saved profile. Overwrites all current settings |
| Delete Profile | Remove a saved profile |
| Rename Profile | Rename an existing profile |
| Default Profile | Designate one profile as the default loaded at login |
| Cross-Device Sync | Profiles sync to account server. Available on any device logged into the account |
| Login Screen Access | Settings profiles are accessible from the login screen (before character selection) for display, audio, and keybinding adjustments |

### Profile Contents

A settings profile captures:

- All keybindings
- All display settings
- All combat settings
- All audio settings
- All gameplay settings
- All notification settings

A settings profile does NOT capture:

- Privacy settings (always per-account, not per-profile)
- Account settings (always per-account)
- Behavioral identity preferences (tied to character identity, not convenience)

---

## Default Configuration

The out-of-box configuration optimized for a new player's first session.

### Default Keybindings

All keybindings listed in the Keybindings section above represent defaults.

### Default Settings Summary

| Category | Key Defaults | Rationale |
|----------|-------------|-----------|
| Display | Medium text, Dark theme, Locked panels, 90% opacity, Expanded mode | Readable without adjustment. Dark theme reduces eye strain for text-heavy UI |
| Combat | Auto-commit in order, Both timer displays, Both log detail, Normal narration | New players see the full combat experience. Mechanical + narrative log teaches system literacy |
| Privacy | All public, all visible | Social-first default encourages community engagement. Players restrict as desired |
| Notifications | All on, 5s toasts, DND off | Nothing missed during early play. Players tune down as they learn what matters |
| Audio | Master 80, Music 70, Combat 80, Ambient 50 | Music and ambient lower to keep combat feedback prominent. Master below 100 leaves headroom |
| Gameplay | Detailed tooltips, Overcharge confirm on, High-risk confirm on, Auto-loot all, Hints on | Safety nets on by default. Experienced players disable confirmations. Auto-loot all prevents missed drops |
| Account | 2FA disabled (prompted at first login) | Prompted but not forced. Reduces onboarding friction |

### First-Time Player Experience

On first login, the following settings-related prompts appear:

1. **Text Size Check** — "Is this text comfortable to read?" with live preview of all 4 sizes
2. **2FA Prompt** — "Secure your account with two-factor authentication" with setup flow or skip option
3. **Keybinding Summary** — one-time overlay showing core keybindings (dismissable, re-accessible from settings)

These prompts do not repeat after dismissal.

---

## Accessibility Notes

| Feature | Purpose |
|---------|---------|
| Full keybinding rebinding | Motor accessibility — any action mappable to any key |
| Text size scaling (4 tiers) | Visual accessibility — readable at all sizes |
| Mono audio | Hearing accessibility — no stereo-dependent audio cues |
| High-contrast theme (Dark default) | Visual accessibility — text-based game requires strong contrast |
| Tooltip detail levels | Cognitive accessibility — information density adjustable |
| Clash narration skip | Cognitive/time accessibility — removes wait time for experienced players |
| Confirmation dialogs (overcharge, high-risk) | Error prevention — safety nets for consequential actions |
| Custom theme contrast check | Ensures player-created themes remain readable (WCAG AA minimum) |

---

## Implementation Priority

| Feature | Priority | MVP? |
|---------|----------|------|
| Keybindings (combat + panels) | Critical | Yes |
| Keybinding rebinding UI | High | Yes |
| Display settings (text size, theme, UI mode) | Critical | Yes |
| Combat settings (auto-commit, log detail, narration speed) | Critical | Yes |
| Audio volume sliders (all channels) | High | Yes |
| Gameplay settings (tooltips, confirmations, auto-loot) | High | Yes |
| Privacy settings (build card, online status, inspect) | High | Yes |
| Notification toggles (per-category) | Medium | Yes (basic) |
| Settings profiles (save/load) | Medium | Post-MVP |
| Cross-device sync | Medium | Post-MVP |
| Custom theme editor | Low | Post-MVP |
| Draggable panel layout | Low | Post-MVP |
| DND scheduled hours | Low | Post-MVP |
| Account settings (2FA, device management) | High | Yes |
| Data export / account deletion | High | Yes (legal requirement) |
| Login screen settings access | Medium | Post-MVP |
| First-time player prompts | High | Yes |

---

## Related Pages

- [[UI Design]]
- [[Combat System]]
- [[Behavioral Identity]]
- [[Cosmetic System]]
- [[Account & Security System]]
- [[Notification & Mail System]]
- [[MVP Design]]
- [[GDD Hub]]
