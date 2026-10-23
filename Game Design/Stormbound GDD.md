---
tags:
  - gdd
  - hub
  - narrative
status: active
created: 2026-10-23
---

# Stormbound — Game Design Document

> **Captain a storm-runner through a shattered archipelago. Every choice changes who sails with you, and what's left of the world when you reach the end.**

A **single-player, choice-driven narrative game** with a strategy layer, played mobile-first on the web. It replaces the MMO design archived in [[GDD Hub|Archive/Stormbound MMO GDD]].

---

## Pillars

1. **Choices with teeth.** Consequences are visible, often delayed, and sometimes irreversible. People leave, factions turn, and things break. See [[Choice & Consequence]].
2. **A crew worth losing.** Six characters you get to know, rely on, and can lose. See [[Crew & Bonds]].
3. **Strategy without combat.** You win by sending the right person to the right problem and spending limited days well. See [[Dispatch Board]] and [[Voyage Calendar]].
4. **Reads like a comic, plays on a phone.** A motion comic with animated panels, one thumb. See [[Motion Comic Presentation]].
5. **Dark, with light in it.** Violence, gore, nudity, grief and nihilism, set against calm, joy, purpose and peace. Mature rating. See [[Tone & Content]].

---

## Core Loop (one in-game day)

| Part of day | What you do | System |
|---|---|---|
| **Dawn** | A story scene with choices | [[Story Structure]] |
| **Watch** | Calls come in across the islands. Send crew, manage risk and time | [[Dispatch Board]] |
| **Dusk** | Spend the evening with one crew member | [[Crew & Bonds]] |
| **Night** | Consequences land, gems shift, the journal updates | [[Choice & Consequence]] |

An **episode** is one voyage of about 7 days and roughly 60-90 minutes of play. A **season** is 5 episodes.

---

## The Premise

The world's old nervous system, the Attunement Lattice, shattered long ago into the gems embedded in everything (see [[Lore & Narrative]]). You are a newly made captain out of Port Haven with a small ship, the *Halyard*, and a debt. On your first salvage job you pull something out of a wreck: a Lattice core that is **waking up**.

The three great factions all want it, for incompatible reasons:

| Faction | Alignment | What they want with the core |
|---|---|---|
| The Luminari | Radiant | Restore the Lattice and freeze the world back into order |
| The Hollow Court | Corrupted | Feed it, accelerate mutation, and let the world become something new |
| The Driftborn | Unbound | Let the core choose for itself |

The core reacts to your choices. By the end of the season, what it becomes is the ending.

---

## What Carried Over From the MMO Design

| Repurposed | How |
|---|---|
| [[Lore & Narrative]], [[Factions System]], [[NPC System]] | World, history, factions, Port Haven and its people |
| [[Alignment System]] | Radiant / Corrupted / Unbound becomes the core's alignment, driven by your choices |
| [[Behavioral Identity]] | Becomes your crew's *read* on you: patterns in your choices that characters comment on |
| Gem domains, stability and mutation ([[Gem Identity System]], [[Combat System#Gem Stability and Mutation]]) | Each crew member is bonded to a gem. Overuse strains it, and strain causes mutations, which become story events |
| The Six Stats ([[Stats System]]) | Crew traits on the Dispatch Board |
| The forecast and plain-language explanations from the combat prototype | The Dispatch Board's success forecast |
| [[PKO Teardown]] lessons | Steal and Avoid lists still apply to progression and monetization |

**Dropped:** MMO servers and networking, the NPC marketplace economy, guilds, PvP, phase-based combat, matchmaking.

---

## MVP: Episode 1, "Salvage Rights"

- 7 in-game days, 6 crew, about 20-25k words.
- 3 episode endings that carry forward as state.
- Playable offline as a PWA, with saves in the browser.
- Success test: playtesters finish the episode, can name a choice they regret, and want episode 2.

See [[Story Structure#Episode 1 — Salvage Rights]].

---

## Notes

| Note | Covers |
|---|---|
| [[Story Structure]] | Season arc, episode format, branching model, Episode 1 beats |
| [[Choice & Consequence]] | State model, consequence types, feedback rules, writing rules |
| [[Crew & Bonds]] | The six crew, traits, bonds, gem strain, losing crew |
| [[Dispatch Board]] | Calls, crew matching, forecast math, outcomes |
| [[Voyage Calendar]] | Day structure, supplies, deadlines |
| [[Motion Comic Presentation]] | Panel language, animation, mobile layout, accessibility, art plan |
| [[Narrative Tech Plan]] | TypeScript + ink, saves, PWA, build order |
| [[Tone & Content]] | Emotional range, rules for dark content, rating, content settings, distribution |
| [[Relationships & Life Events]] | Friendship, rivalry, romance, marriage, divorce, jail, other life events |
| [[Professions & Advancement]] | Professions, tiers, branching story-decided Paths for each crew member and the captain |
| [[Workshop & Crafts]] | Botany, chemistry and smithing aboard ship: crafted items for calls, story choices, gem strain, the psyche and jailbreaks |
| [[Peoples, Clans & Cast]] | The archipelago's peoples (Islanders, Tidefolk, Shardkin, the Quieted, Wakened), clans, crew backgrounds, the relationship web |
| [[Lineage, Pacts & Diplomacy]] | Family trees and inheritance, cultural traditions, blood pacts, marriage alliances, fosterage, treaties; kinship blocks romance |
| [[Psyche & Trauma]] | Stress, wounds and triggers, breaking points, the captain's unreliable mind; the main source of unpredictability |

## Open Questions

- Business model: premium per season, or episode 1 free and the rest paid? (No pay-to-win either way.)
- Captain customization: name and pronouns only, or a chosen background as well?
- Illustration budget: typographic comic only, or commissioned character portraits? (See [[Motion Comic Presentation#Art Plan]].)
