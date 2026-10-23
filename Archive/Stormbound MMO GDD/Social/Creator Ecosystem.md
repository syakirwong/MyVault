---
tags:
  - gdd
  - social
  - creators
---

> [!warning] Archived 2026-10-23
> Part of the Stormbound **MMO** design, superseded when the game pivoted to a single-player narrative game. See [[Stormbound GDD]]. Kept as source material.

# Creator Ecosystem

---

## Vision

Players become build creators -- theorycrafters who publish, iterate, and gain recognition for their builds. This is the long-term content engine.

---

## Build Publishing System

- Any player can publish a build (Origin + gem loadout + modifiers + alignment + gear recommendations).
- Build gets a unique Build Code (shareable string, e.g., "DUE-STM-CRT-PHZ-COR").
- Importing a build code auto-equips matching gems (if owned).
- Published builds track: usage count, win rates, clear times, popularity over time.

---

## Build Lab & Public Database

> [!info] Source
> PKO's community runs on a public item and monster database plus a build simulator ([[PKO - Tech & Infrastructure]]). For Stormbound these are core features, not fan extras.

### Build Lab (web)

- Usable without logging in. Logging in is only needed to publish or to pull your owned gems.
- Paste a Build Code to get the full stat sheet, power-budget breakdown, breakpoint markers and DPS model, computed by the same [[Technical Architecture#Shared Rules Package|Shared Rules Package]] as the server.
- Theorycraft with gems you don't own. A ghost-slot marks unowned gems so the build still exports as a code.
- Side-by-side compare (powers the comparison tool below) and one-click Fork.

### Public Game Database

- Every gem, modifier, origin, monster and dungeon, generated from the shipped game data each release.
- Deep links double as chat item links ([[Chat & Communication System#Item Linking]]).
- **Discovery guard:** Mythic dungeon mechanics and boss scripts are withheld from the database until the first server-wide clear. PKO publishes everything, so static content gets solved on day one.

---

## Creator Profiles

- Public profile page showing all published builds.
- Stats: Total users of your builds, average rating, top build.
- Creator level based on total impact (usage * rating).
- Creator level unlocks:
  - **Level 1:** Publish builds (requires gem mastery 10 in any gem)
  - **Level 5:** Featured on "New Creators" page
  - **Level 10:** Custom build thumbnail
  - **Level 20:** Creator title in-game
  - **Level 50:** Creator cosmetic set (exclusive)

---

## Leaderboards

- **Most Used Builds** (this week / this month / all time)
- **Fastest Dungeon Clears** (per build archetype)
- **Top PvP Builds** (by win rate, minimum 100 matches)
- **Most Innovative** (community voted, weekly)
- **Rising Stars** (fastest growing new builds)

---

## Community Features

- Build comments and ratings
- Build guides (text + screenshot support)
- Build comparison tool (side-by-side stats)
- "Fork" a build -- create a variant of someone else's build (credits original)
- Build patch notes (creators can update builds and log changes)

---

## Creator Rewards

- Monthly top creators get exclusive cosmetic rewards.
- Creator currency earned from build usage (spendable on cosmetics).
- Community spotlight in launcher/loading screens.
- Invitation to beta testing for balance patches.

---

## Related Pages

- [[Gem System]]
- [[UI Design]]
- [[Social Hubs]]
