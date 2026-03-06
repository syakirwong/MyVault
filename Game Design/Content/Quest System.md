---
tags:
  - gdd
  - content
  - quests
---

# Quest System

## Design Philosophy

Quests teach systems, reveal lore, and provide structured goals. They are never busywork. Every quest exists because it introduces a mechanic, advances a story, or rewards mastery of a system the player already understands.

If a quest could be replaced with "kill 10 boars" and nothing of value would be lost, the quest should not exist.

Three rules govern quest design:
1. **Every quest teaches or tests something** — a system, a build concept, a narrative choice, a resonance interaction
2. **Rewards match effort and stakes** — trivial quests give trivial rewards; meaningful quests give meaningful ones
3. **Player agency matters** — where possible, quests offer choices that affect outcomes, alignment, or faction standing

---

## Quest Types

### Story Quests (Main Narrative)

The primary narrative thread connecting regions, factions, and the world's central conflict.

| Property | Detail |
|----------|--------|
| Structure | Multi-step chains with branching at key decisions |
| Branching | Alignment choices fork quest outcomes. Some branches are exclusive (choosing one locks the other) |
| Frequency | Fixed quantity per region. Not repeatable |
| Rewards | Highest XP (1,000-3,000 per quest), guaranteed gems at key milestones, alignment depth, faction reputation, codex entries |
| Sharing | **Not shareable.** Story quests are personal narrative progression |

**MVP Story Chains:**

| Chain | Region | NPC | Length | Alignment Tie |
|-------|--------|-----|--------|---------------|
| "The First Spark" | Calm Shores | [[NPC System#Captain Maren Voss\|Captain Maren Voss]] | 8-10 quests (~2 hours) | None (tutorial) |
| "Trial by Storm" | Stormreach | [[NPC System#Admiral Kael Stormwright\|Admiral Kael Stormwright]] | 10-12 quests (~3 hours) | Introduces alignment choice |
| "The Ascending Light" | Celestial Peaks | [[NPC System#High Luminar Sera\|High Luminar Sera]] | 12-15 quests (~4 hours) | Radiant |
| "The Price of Power" | Obsidian Wastes | [[NPC System#The Hollow King\|The Hollow King]] | 12-15 quests (~4 hours) | Corrupted |
| "The Third Path" | Verdant Depths | [[NPC System#The Driftmind\|The Driftmind]] | 12-15 quests (~4 hours) | Unbound |

### Side Quests

NPC-specific stories that deepen world lore and teach secondary systems. Each named NPC with a questline offers 5-8 side quests.

| Property | Detail |
|----------|--------|
| Structure | Linear chains (3-8 steps) tied to a specific NPC |
| Branching | Minimal. Occasional alignment-flavored dialogue but no major forks |
| Frequency | Fixed per NPC. Not repeatable |
| Rewards | Moderate XP (200-800), crafting recipes, NPC-specific unlocks, faction reputation, codex entries |
| Sharing | **Not shareable.** Side quest progress is individual |

**Examples:**

| NPC | Side Quest Chain | Teaches |
|-----|-----------------|---------|
| [[NPC System#Tova Ironhand\|Tova Ironhand]] | "The Perfect Cut" (5 quests) | Gem Shaping crafting discipline |
| [[NPC System#Lumen\|Lumen]] | "Resonance Theory" (6 quests) | Resonance Mapping, resonance discovery mechanics |
| [[NPC System#Vaela Thornstitch\|Vaela Thornstitch]] | "Roots and Reagents" (5 quests) | Catalyst Brewing, mutation catalysts |
| [[NPC System#Arena Warden Zara\|Arena Warden Zara]] | "Proving Grounds" (4 quests) | PvP mechanics, momentum mastery, conditional actions |

### Daily Bounties

Short, rotating objectives that provide consistent daily engagement. See [[#Daily Bounties Detail]].

| Property | Detail |
|----------|--------|
| Structure | Single-objective tasks. Complete and turn in |
| Frequency | 3 available per day. Pool rotates daily |
| Rewards | XP (300-500), gold (50-150), faction reputation (100-200 for regional faction) |
| Sharing | **Shareable.** Party members working on the same bounty share progress |
| Repeatable | Daily reset. Same bounty may appear on different days |

### Alignment Quests

Content gated by alignment choice and depth. These quests explore the philosophical dimensions of each alignment and offer moral decisions that deepen or challenge the player's commitment.

| Property | Detail |
|----------|--------|
| Structure | Multi-step chains with moral choice points |
| Branching | Every alignment quest includes at least one choice that could shift alignment depth (+5 to +15 depth on commitment, -5 to -10 on doubt) |
| Frequency | Unlocked at depth thresholds (see [[Alignment System#Depth Tiers]]). Not repeatable |
| Rewards | Alignment depth, exclusive gems (at Zealot tier), alignment-specific cosmetics, story codex entries |
| Sharing | **Not shareable.** Alignment choices are personal |
| Prerequisite | Alignment depth 21+ (Devoted tier). Higher-tier quests require deeper commitment |

### Faction Quests

Repeatable quests tied to faction reputation progression. Each faction offers structured tasks that reflect its identity. See [[Factions System#Faction Daily Quests]] for the daily variant.

| Property | Detail |
|----------|--------|
| Structure | Single or multi-step objectives aligned with faction theme |
| Frequency | Weekly rotation (new set each Monday). Repeatable weekly |
| Rewards | Faction reputation (300-800), gold, faction-specific materials, recipe unlocks at reputation thresholds |
| Sharing | **Shareable** for group-oriented factions (Arena League match quests, Trade Guild escort quests). **Not shareable** for solo-oriented tasks (Gemcutter's Circle crafting quests) |

**Examples by Faction:**

| Faction | Weekly Quest Example | Objective |
|---------|---------------------|-----------|
| Luminari | "Shield the Pilgrims" | Escort NPC caravan through contested zone without losing any pilgrims |
| Hollow Court | "Corruption Harvest" | Collect 5 void essence from Rift Zone enemies |
| Driftborn | "The Mediator's Work" | Complete a cross-faction NPC dispute chain (3 dialogue encounters) |
| Trade Guild | "Market Maker" | Complete 5 NPC marketplace transactions in a single day |
| Arena League | "Ranked Dedication" | Win 5 ranked PvP matches this week |
| Gemcutter's Circle | "Experimental Batch" | Craft 5 items of Rare+ quality using a recipe you have not used before |

### Discovery Quests

Hidden quests triggered by exploration. These are never shown in the quest log until discovered. They reward curiosity and thorough world engagement.

| Property | Detail |
|----------|--------|
| Structure | Triggered by: entering a hidden area, interacting with an unmarked object, finding a lore fragment, or defeating a rare ambient enemy |
| Frequency | Fixed. Not repeatable. One-time discovery per character |
| Rewards | XP (100-500), cosmetic unlocks, codex entries, resonance hints, rare gem drops |
| Sharing | **Not shareable.** Discovery is individual |

**Discovery triggers are never telegraphed.** No quest markers, no NPC hints. The reward is the discovery itself. The quest log entry appears only after the trigger condition is met.

### Resonance Quests

Triggered when a player discovers a specific resonance for the first time. These quests guide players deeper into the resonance system by asking them to reproduce, refine, or weaponize the discovered resonance.

| Property | Detail |
|----------|--------|
| Structure | 2-4 step chain: discover resonance (trigger) -> reproduce it in combat -> achieve a specific outcome using it |
| Frequency | One per resonance discovery. Not repeatable |
| Rewards | XP (250 flat per [[Experience & Levelling#XP Sources]]), resonance tokens, resonance hints for related domain pairs, codex entry |
| Sharing | **Not shareable.** Resonance discovery is personal |
| Prerequisite | Must have naturally triggered the resonance in combat. Cannot be granted by another player |

---

## Quest Log UI

### Active Quest Limit

**15 active quests maximum.** Breakdown by category:

| Category | Reserved Slots | Notes |
|----------|---------------|-------|
| Story Quests | Up to 3 | Only 1 main story chain active at a time, but multi-step chains may occupy 1-3 slots |
| Side Quests | Up to 5 | NPC chains |
| Daily Bounties | 3 | Fixed daily cap per [[Experience & Levelling#XP Sources]] |
| Alignment / Faction / Other | Remaining | Flexible allocation |

If a player is at 15 active quests, new quests cannot be accepted until one is completed or abandoned. Abandoned quests can be re-accepted from the original NPC with no penalty (progress resets).

### Tracking and Pinning

- **1 pinned quest** at a time. Pinned quest shows objectives in the persistent HUD
- **Quest compass** — directional indicator toward the pinned quest's next objective (text-based: "Objective: Northeast, Stormreach docks")
- **Auto-pin** — newly accepted quests auto-pin unless the player has manually pinned another quest

### Quest Log Tabs

| Tab | Contents |
|-----|----------|
| **Active** | All in-progress quests, sorted by category |
| **Completed** | Completed quest history with reward summary |
| **Bounties** | Daily bounty board (3 active, shows remaining time until reset) |
| **Codex** | Lore entries unlocked through quest completion |

### Completion Indicators

| State | Visual |
|-------|--------|
| Available (not yet accepted) | NPC nameplate shows quest marker (gold for story, silver for side, blue for faction) |
| In Progress | Quest log entry with checkbox objectives. Partial completion shown (e.g., "Defeat Storm Elementals: 2/5") |
| Ready to Turn In | NPC nameplate shows completion marker (green). Quest log entry highlighted |
| Completed | Moved to Completed tab. NPC marker removed |

---

## Quest Structure

### Objective Types

| Objective Type | Description | Example |
|----------------|-------------|---------|
| **Defeat** | Kill specific enemies or enemy types | "Defeat 5 Storm Elementals in Stormreach" |
| **Collect** | Gather items from enemies, nodes, or the world | "Collect 3 Void Essence from Rift Zone enemies" |
| **Explore** | Reach a specific location or discover a hidden area | "Find the abandoned lighthouse on the northern cliffs" |
| **Craft** | Create a specific item or craft using a discipline | "Craft a Rare Tempest gem using Gem Shaping" |
| **Reach Location** | Travel to a designated area | "Report to Admiral Kael at Stormreach Command Tower" |
| **Make Choice** | Alignment or narrative decision point | "Choose: Spare the Corrupted scout (-5 Corrupted depth) or Execute (+10 Corrupted depth)" |
| **Combat Condition** | Achieve a specific combat state during a fight | "Win a fight with Momentum 9+" or "Trigger a resonance during a boss encounter" |
| **Dialogue** | Speak with an NPC, select dialogue options | "Ask Lumen about the Storm Leviathan's gem ecology" |
| **Escort** | Protect an NPC or object during transit | "Escort the trade caravan from Port Haven to Stormreach" |
| **Survive** | Endure a timed or wave-based encounter | "Survive 10 rounds against the training construct" |

### Branching

Story quests and alignment quests support branching at **choice nodes**.

| Branch Type | Mechanic | Consequence |
|-------------|----------|-------------|
| **Alignment Fork** | Player chooses an action aligned with Radiant, Corrupted, or Unbound | Affects alignment depth. May lock or unlock subsequent quests. Changes NPC dialogue |
| **Narrative Fork** | Player chooses between two outcomes (e.g., save village vs. pursue enemy) | Determines which follow-up quest is offered. Locked choice cannot be revisited |
| **Reward Fork** | Player chooses their reward from 2-3 options | Typically: gem vs. gold vs. crafting materials. Choice reflects player priorities |

### Multi-Step Chains

Story and side quest chains follow this pattern:

```
Quest 1: Introduction (meet NPC, learn context)
  -> Quest 2-3: Rising action (objectives that teach relevant systems)
    -> Quest 4-5: Complication (higher difficulty, narrative tension)
      -> Quest 6+: Climax (boss encounter, major choice, or revelation)
        -> Resolution (rewards, narrative payoff, next chain tease)
```

### Prerequisites

| Prerequisite Type | Example |
|-------------------|---------|
| Level | "Requires Level 15" (Stormreach access) |
| Prior Quest | "Requires completion of 'The First Spark'" |
| Alignment Depth | "Requires Radiant Depth 21+" |
| Faction Reputation | "Requires Gemcutter's Circle: Trusted" |
| Gem Mastery | "Requires any gem at Mastery 5+" |
| Discovery | "Requires discovery of [hidden location]" |

---

## Quest Rewards

All quest XP values align with the [[Experience & Levelling#XP Sources]] table. Gold values align with the [[Economy Model]] source rates.

### Reward Table by Quest Type

| Quest Type | XP | Gold | Gems | Materials | Alignment Depth | Faction Rep | Cosmetics | Codex |
|------------|-----|------|------|-----------|----------------|-------------|-----------|-------|
| Story Quest | 1,000-3,000 | 100-500 | Guaranteed at milestones | Occasional | +5 to +15 at choice nodes | 500-1,500 | At chain completion | Yes |
| Side Quest | 200-800 | 50-200 | Rare | Crafting recipes | None | 100-500 | Occasional | Yes |
| Daily Bounty | 300-500 | 50-150 | No | Occasional | None | 100-200 | No | No |
| Alignment Quest | 400-1,000 | 100-300 | Exclusive (at Zealot tier) | None | +5 to +15 | 200-500 | At depth milestones | Yes |
| Faction Quest | 300-600 | 100-300 | No | Faction-specific | None | 300-800 | At rep tier thresholds | Occasional |
| Discovery Quest | 100-500 | 50-200 | Rare | None | None | None | Occasional | Yes |
| Resonance Quest | 250 (flat) | 50 | No | None | None | None | No | Yes |

### Guaranteed Gem Rewards

Key story milestones guarantee specific gems to ensure build diversity is taught, not left to luck.

| Quest Milestone | Guaranteed Gem |
|-----------------|---------------|
| "The First Spark" completion (tutorial) | Player's first Skill Gem (domain chosen by player from 3 options) |
| First dungeon clear (Tidecaller Caverns) | Uncommon gem matching dungeon's domain ecology (Aegis) |
| "Trial by Storm" midpoint | Rare Tempest or Kinetics gem (player choice) |
| Alignment quest (Zealot tier, depth 41+) | Alignment-exclusive gem (see [[Alignment System#Depth Tiers]]) |

### Resonance Hints

Some quest rewards include **resonance hints** — cryptic clues about undiscovered resonance pairings. These appear as codex entries:

> *"When the storm answers the void, something stirs between them."*
> (Hints at a Tempest + Void resonance)

Hints are never explicit. They guide experimentation without revealing the answer.

---

## Daily Bounties Detail

Daily bounties are the primary short-session engagement tool. They are referenced in [[Experience & Levelling#XP Sources]] (capped at 3/day for XP) and [[Gameplay Loops#Session Loop]].

### Structure

| Property | Detail |
|----------|--------|
| Available per day | 3 bounties |
| Selection | Drawn from a rotating pool. Player sees 3 and must complete those 3 (no rerolling at MVP) |
| Duration per bounty | 10-20 minutes |
| Reset time | Server daily reset (midnight UTC) |
| Incomplete bounties | Expire at reset. Progress does not carry over |

### Bounty Pool

Bounties draw from the following objective categories, weighted toward the player's current region:

| Category | Example Bounties | Weight |
|----------|-----------------|--------|
| Combat | "Defeat 10 enemies in [current region]" | 30% |
| Dungeon | "Complete any Farming dungeon" | 20% |
| Crafting | "Craft 2 items of Uncommon+ quality" | 15% |
| Exploration | "Discover 3 resource nodes in [current region]" | 15% |
| PvP | "Complete 2 PvP matches (win or lose)" | 10% |
| Social | "Inspect 3 other players' builds" | 10% |

### Scaling Rewards

Bounty rewards scale with player level to remain relevant through the leveling curve and at endgame.

| Level Range | XP per Bounty | Gold per Bounty |
|-------------|--------------|----------------|
| 1-15 | 300 | 50 |
| 16-30 | 400 | 100 |
| 31-50 | 500 | 150 |
| 50+ (Prestige) | 500 (diminished toward prestige XP) | 150 |

### Bounty Board

Located in every [[Social Hubs|Social Hub]]. Accessible from Dungeon Guide Pip in dungeon entrances. Shows:
- Today's 3 bounties with objectives and rewards
- Completion progress for each
- Time until daily reset

---

## Quest Sharing

| Quest Type | Shareable? | Shared Progress? | Notes |
|------------|-----------|-----------------|-------|
| Story Quest | No | No | Personal narrative. Each player experiences their own story |
| Side Quest | No | No | NPC relationship is individual |
| Daily Bounty | **Yes** | **Yes** | Party members working on the same bounty share kill/collect progress. All must have the bounty active |
| Alignment Quest | No | No | Alignment choices are personal and consequential |
| Faction Quest | **Conditional** | **Conditional** | Group-oriented faction quests (escort, PvP) share progress. Solo-oriented (crafting, appraisal) do not |
| Discovery Quest | No | No | Discovery is individual. Being in a party does not auto-trigger discovery for others |
| Resonance Quest | No | No | Resonance discovery is personal |

**Shared progress rules:**
- All party members must have the same quest active
- Kill/collect objectives count for all members present in combat or within range
- Turn-in is individual — each player must return to the NPC

---

## Repeatable Content

| Quest Type | Repeatable? | Reset Frequency | Diminishing Returns? |
|------------|-----------|----------------|---------------------|
| Story Quest | **Never** | N/A | N/A |
| Side Quest | **Never** | N/A | N/A |
| Daily Bounty | **Daily** | Midnight UTC | No diminishing returns (capped at 3/day instead) |
| Alignment Quest | **Never** | N/A | N/A |
| Faction Quest (Daily) | **Daily** | Midnight UTC | No (rep-gated by tier thresholds) |
| Faction Quest (Weekly) | **Weekly** | Monday UTC | XP diminished by 50% after 3rd completion of the same quest. Rep reward unchanged |
| Discovery Quest | **Never** | N/A | N/A |
| Resonance Quest | **Never** | N/A | N/A |

**Diminishing XP for repeated content** follows the rule defined in [[Experience & Levelling#Anti-Exploit Rules]]: same dungeon in the same session grants 100% -> 75% -> 50% -> 25% -> 10%. This applies to quest-associated dungeon clears as well. Faction weekly quests additionally diminish XP (not rep) after the 3rd weekly completion of the same objective to prevent pure rep-farming from dominating XP gains.

---

## Quest and Onboarding Integration

The tutorial questline ("The First Spark") is the entry point for the [[Onboarding System]]. It sequences system introductions:

| Quest Step | System Introduced |
|------------|-------------------|
| 1. "A New Arrival" | Movement, navigation, NPC interaction |
| 2. "The Gem Awakens" | First gem acquisition, gem equipping |
| 3. "Trial of Sparks" | Combat phases (Draw, Commit, Clash, Aftermath) |
| 4. "Reading the Storm" | Combat log reading, enemy intent |
| 5. "Second Nature" | Second gem slot, multi-domain loadout |
| 6. "The Shifting Edge" | Gem stability, mutation, Overcharge |
| 7. "Echoes Between" | Resonance discovery |
| 8. "Into the Deep" | First dungeon (Tidecaller Caverns), party formation |
| 9. "The Builder's Eye" | Crafting introduction (Tova Ironhand) |
| 10. "Port Haven Awaits" | Social hub, marketplace, build publishing |

Each step is designed to feel like gameplay, not a tutorial. The player is advancing a narrative (Captain Maren Voss's story) while learning systems.

---

## MVP Quest Scope

Aligned with [[MVP Design]] (Calm Shores + Stormreach):

| Quest Type | Count | Notes |
|------------|-------|-------|
| Story Quests | ~22 | "The First Spark" (10) + "Trial by Storm" (12) |
| Side Quests | ~25 | 5 NPC chains x ~5 quests each (Tova, Lumen, Holt, Zara, Maren post-tutorial) |
| Daily Bounties | 15-20 in rotation pool | 3 active per day, rotating |
| Alignment Quests | 6-9 | 2-3 per alignment (Initiate and Devoted tiers only at MVP, depth 0-40) |
| Faction Quests | 12-18 | 2-3 per faction (6 factions), weekly rotation |
| Discovery Quests | 8-12 | Hidden across Calm Shores and Stormreach |
| Resonance Quests | ~10 | Triggered by first resonance discoveries |
| **Total** | **~100** | |

---

## Related Pages

- [[Combat System]] — Combat conditions as quest objectives
- [[PvE Structure]] — Dungeons as quest destinations
- [[PvP Structure]] — PvP match quests, arena progression
- [[NPC System]] — Quest-giving NPCs, dialogue design
- [[Alignment System]] — Alignment quests, depth progression
- [[Factions System]] — Faction quests, reputation sources
- [[Experience & Levelling]] — XP sources, daily bounty cap, diminishing returns
- [[Gameplay Loops]] — Session, weekly, and meta loop integration
- [[World Design]] — Quest locations, exploration triggers
- [[Onboarding System]] — Tutorial questline integration
- [[Death & Defeat System]] — Quest progress persistence through death
- [[Economy Model]] — Gold reward rates, crafting quest costs
- [[Gem Identity System]] — Resonance quests, gem rewards
- [[Creator Ecosystem]] — Build publishing unlocked via quest milestone
