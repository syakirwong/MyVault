---
tags:
  - gdd
  - ui
  - cosmetics
  - monetization
---

# Cosmetic System

In a text-based interface, identity is expressed through text styling, symbols, badges, titles, and UI customization — not character models or skins.

---

## Cosmetic Categories

| Category | Source | Tradeable | Expression |
|----------|--------|-----------|------------|
| Name Colors | Gameplay (mastery, rarity) | No | Colored player name in all contexts |
| Alignment Badges | Gameplay (alignment depth) | No | Symbol next to name plate |
| Title Prefixes | Achievements only | No | Text before player name |
| Title Suffixes | Cash shop + Achievements | Mixed | Text after player name |
| Chat Flair | Cash shop + Quests | No | Custom chat message borders/colors |
| Build Card Borders | Gameplay (mastery) + Cash shop | No | Border style on build display |
| Emote Commands | Cash shop + Quests | No | Custom emote text in chat/log |
| Profile Banners | Cash shop + Guild achievements | No | Profile page header style |
| Combat Log Style | Cash shop | No | Custom combat log text effects |

---

## Design Rules

1. **Gameplay-earned cosmetics are ALWAYS more impressive than cash shop.** A legendary mastery name color outshines any purchased flair.
2. Cash shop cosmetics are stylistic variations, not upgrades.
3. No cash shop item mimics a high-tier gameplay indicator (prevents confusion).
4. Earned titles displayed more prominently than purchased cosmetics.
5. Build identity (gems + alignment indicators) is never overridden by cosmetics — cosmetics layer ON TOP.

---

## Name Plate Anatomy

```
[Radiant] ★★★ Tactician Stormweaver Kael, Resonance Explorer
   ↑        ↑      ↑          ↑       ↑          ↑
alignment mastery behavioral  gem     name     milestone
 badge    stars  archetype  archetype          suffix
```

- **Alignment badge**: Earned through alignment depth. Color-coded.
- **Mastery stars**: 1-5 stars based on gem mastery achievements.
- **Behavioral archetype**: Dynamic title from the [[Behavioral Identity]] system. Reflects how you play, not what you equip. Updates as playstyle shifts.
- **Gem archetype**: Generated from equipped gem domains (see [[Gem Identity System]]).
- **Player name**: Base color determined by highest gem rarity mastered.
- **Milestone suffix**: Earned through behavioral and achievement milestones (see [[Behavioral Identity]]). Permanent once earned.

See [[Behavioral Identity]] for the full system that drives behavioral titles, combat log voice, hit effects, aura generation, and build card evolution.

---

## Name Color Progression

| Achievement | Name Color |
|-------------|-----------|
| Default | White |
| First Uncommon gem mastered | Light Blue |
| First Rare gem mastered | Blue |
| First Epic gem mastered | Purple |
| First Legendary gem mastered | Gold |
| All 4 slots Legendary mastered | Gold + shimmer effect |

---

## Cash Shop Pricing (reference, not final)

| Item | Price |
|------|-------|
| Title suffix | $3-5 |
| Chat flair set | $5-8 |
| Profile banner | $5-8 |
| Build card border | $3-5 |
| Emote pack (5) | $5 |
| Combat log style | $3-5 |
| Battle pass (seasonal, cosmetic only) | $10/season |

---

## Build Card Display

How a player's build appears when inspected or shared:

```
╔══════════════════════════════════════╗
║  ★★★ Stormweaver Kael               ║
║  [Corrupted] Stormborn  Lv 47       ║
╠══════════════════════════════════════╣
║  SKILLS                              ║
║  ⚡ Storm Strike [Corrupted]          ║
║    ├ Overload ★★                     ║
║    └ Chain Reaction ★                ║
║  ⚔ Lightning Chain                    ║
║    ├ Sharpened Edge ★★★              ║
║    └ Combo Finisher ★★              ║
║  ◈ Shift Strike [Corrupted]          ║
║    ├ Quicksilver ★                   ║
║    └ Void Touch ★★                   ║
║  ⚡ Force Strike                       ║
║    ├ Elemental Focus ★★              ║
║    └ AoE Conversion ★               ║
╠══════════════════════════════════════╣
║  DPS: 1,847  SURV: 42  UTIL: 18     ║
║  BUILD CODE: SB-DUE-COR-7x9k2      ║
╚══════════════════════════════════════╝
```

---

## Related Pages

- [[Behavioral Identity]] — The behavioral fingerprinting system that drives dynamic titles, aura, combat log voice, and PvE effects
- [[UI Design]]
- [[Monetization]]
- [[Creator Ecosystem]]
