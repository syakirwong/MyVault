---
tags:
  - gdd
  - ui
  - design
---

# UI Design

How the reactive text-based interface communicates game state, build identity, and combat flow.

---

## Design Principles

1. **Clarity Over Decoration** — every UI element communicates specific information. No ornamental noise.
2. **Reactive Feedback** — actions produce immediate, visible responses in panels and logs.
3. **Build Readability** — a player's build should be identifiable from their UI elements, text colors, and symbols.
4. **Information Density** — show what matters. Hide what doesn't. Progressive disclosure for complexity.

---

## Interface Panels

### Combat Panel

The primary gameplay interface during encounters.

| Element | Purpose |
|---------|---------|
| **Action Bar** | 4 skill gem buttons + normal attack. Cooldown overlays, resource cost display |
| **Enemy Intent** | Shows enemy's next action (telegraph), cast bar, and vulnerability windows |
| **Combat Log** | Scrolling text log of all actions, damage numbers, status effects |
| **Status Bar** | HP, Integrity, active buffs/debuffs with duration timers |
| **Positioning Indicator** | Text-based facing/position relative to target (Frontal / Flank / Behind) |

### Hub Panel

Social and navigation interface in non-combat areas.

| Element | Purpose |
|---------|---------|
| **Location Header** | Current zone name, region, ambient description |
| **Action Buttons** | Context-sensitive: Trade, Craft, Queue Dungeon, Inspect, Duel |
| **Player List** | Visible players with Origin icon, alignment badge, build summary |
| **Chat / Social** | Region chat, party chat, guild chat, whispers |
| **Notifications** | NPC marketplace sales, dungeon ready, build comments, guild events |

### Build Panel

The gem loadout and customization interface.

| Element | Purpose |
|---------|---------|
| **Skill Gem Slots (4)** | Equipped skill gems with modifier slots visible |
| **Modifier Slots (2-3 per gem)** | Attached modifiers with synergy indicators |
| **Build Stats** | Calculated DPS, survivability, utility scores |
| **Build Code** | Shareable build string for creator ecosystem |
| **Alignment Indicator** | Current alignment + depth tier with color coding |

---

## Text Color Language

Colors communicate game information consistently across all panels.

### Element Colors (Damage/Skill Type)

| Element | Color | Hex |
|---------|-------|-----|
| Fire | Orange-Red | `#FF4500` |
| Storm | Electric Blue | `#00BFFF` |
| Nature | Green | `#32CD32` |
| Light | White-Gold | `#FFD700` |
| Void | Deep Purple | `#6A0DAD` |
| Water | Cyan | `#00CED1` |
| Wind | Silver | `#C0C0C0` |
| Physical | Steel Gray | `#708090` |
| Earth | Brown-Amber | `#FFBF00` |

### UI Semantic Colors

| Meaning | Color | Usage |
|---------|-------|-------|
| Damage dealt | Red | Combat log, damage numbers |
| Healing | Green | Combat log, heal numbers |
| Buff gained | Cyan | Status bar, combat log |
| Debuff applied | Orange | Status bar, combat log |
| Critical hit | Gold + bold | Combat log emphasis |
| System message | Gray | Notifications, system events |
| Player name | White | Default player reference |
| NPC dialogue | Light purple | Quest text, NPC interaction |

### Alignment Colors

| Alignment | Primary | Accent | Applied To |
|-----------|---------|--------|------------|
| Radiant | Warm Gold `#FFD700` | White | Name plate, skill text, build border |
| Corrupted | Dark Purple `#6A0DAD` | Crimson `#DC143C` | Name plate, skill text, build border |
| Unbound | Cool Silver `#C0C0C0` | Pale Blue `#B0C4DE` | Name plate, skill text, build border |

---

## Rarity Indicators

Text formatting conveys gem and item rarity without graphics.

| Rarity | Text Style | Symbol |
|--------|-----------|--------|
| Common | Plain text | -- |
| Uncommon | **Bold** | * |
| Rare | **Bold** + blue color | ** |
| Epic | **Bold** + purple color + border | *** |
| Legendary | **Bold** + gold color + glow effect + border | **** |

---

## Combat Log Format

> [!info] Full Specification
> See [[Combat Logs System]] for the complete log structure, narrative signal language, information tiers, and pattern detection system.

The combat log follows the phase-based round structure (Draw → Commit → Clash → Aftermath). Each round is a self-contained log block:

```
══ ROUND 7 ══════════════════════════
CLASH ─────────────────────────────
[R7] You commit: Storm Strike → Corrode → Consume
[R7] Opponent commits: Fortify → Reflect → Construct

  SLOT 1:
  ▸ You: Storm Strike → Opponent (187 Tempest damage)
    ↳ Chain stack: 3 → 4
  ▸ Opponent: Fortify (50% reduction active)
    ↳ Damage reduced: 187 → 94

  ROUND RESULT: You dealt 299, took 94
  Integrity: You 78/100 | Opponent 71/100
──────────────────────────────────
```

---

## Enemy Intent Display

Enemies telegraph their actions through the UI, replacing animation tells.

```
┌─────────────────────────────────────┐
│  STORMCLAW VIPER         HP ████░░  │
│  ──────────────────────────────────  │
│  INTENT: Tail Sweep (AoE)           │
│  CAST:   ████████░░░░  1.2s         │
│  HINT:   Dodge or move behind       │
└─────────────────────────────────────┘
```

---

## Synergy Indicators

When gems share elements or create synergies, the UI reflects this.

| Condition | UI Indicator |
|-----------|-------------|
| 2 matching element gems | Element tag on build card |
| 3 matching element gems | Element border on build card |
| 4 matching (full set) | Signature element title prefix |
| Cross-element synergy | Blended color on synergy skill text |

---

## Responsive Feedback

Every player action produces immediate UI response:

- **Skill cast** → button grays with cooldown timer, combat log entry, damage/effect numbers
- **Dodge success** → flash highlight on status bar, "Dodged!" in combat log
- **Parry success** → gold flash, "Parried!" + stagger applied in combat log
- **Combo chain** → chain counter appears, multiplier shown
- **Resource change** → resource bar animates, +/- number floats
- **Level up / Mastery** → notification banner, stats updated

---

## Related Pages

- [[Gem System]]
- [[Cosmetic System]]
- [[Alignment System]]
- [[Combat System]]
