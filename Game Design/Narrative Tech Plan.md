---
tags:
  - gdd
  - technical
status: active
---

# Narrative Tech Plan

Replaces the archived [[Web Build Plan]] (MMO). **No server at MVP.** The game runs entirely in the browser.

## Stack

| Layer | Choice | Why |
|---|---|---|
| Language | TypeScript | Already chosen ([[Web Build Plan]]) |
| UI | Svelte (SvelteKit, static build) | Fast animated panels, small bundle |
| Story scripting | **ink** (inkle's open-source narrative language) with the **inkjs** runtime | Built for branching stories. Writers work in plain text files, and game state is shared with code through ink variables |
| Game systems | Plain TypeScript modules: dispatch, calendar, strain, loyalty | Pure functions with a seeded RNG, so they're testable and replayable |
| Animation | CSS animations + the Web Animations API | Enough for the panel language; no game engine needed |
| Saves | IndexedDB, plus an export/import save code | Works offline; players can move saves between devices |
| Distribution | PWA first. Later: Capacitor for iOS/Android stores, Tauri for desktop | One codebase |

## How Story and Systems Connect

```text
ink scene  ──sets──▶  variables (flags, loyalty, alignment)
    ▲                         │
    └──reads─── TypeScript systems (dispatch results, strain events)
```

- ink owns **what is said and which choices exist**.
- TypeScript owns **the numbers**: forecast, strain, supplies.
- Each gets the other's state through ink variables and external functions, which ink supports natively.

## Build Order

| Step | Deliverable | Done when |
|---|---|---|
| 1 | Comic panel renderer + one ink scene | A Dawn scene plays with choices, animated panels and saves |
| 2 | Dispatch Board module | One Watch with 4 calls, forecast and outcomes |
| 3 | Day loop | Dawn → Watch → Dusk → Night, with autosave |
| 4 | Episode 1 vertical slice | Days 1-3 written and playable |
| 5 | Full Episode 1 | All 7 days, 3 endings, Consequence Ledger |
| 6 | PWA polish, accessibility settings, playtest build | Installable, works offline |

Related: [[Stormbound GDD]] · [[Motion Comic Presentation]]
