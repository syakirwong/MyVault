---
tags:
  - gdd
  - core
  - combat
  - death
---

# Death & Defeat System

## Design Philosophy

Death is a **learning signal**, not a punishment wall. Consequence scales with the stakes of the content. A player who dies in a normal dungeon should feel inconvenienced, not devastated. A player who dies in a Mythic dungeon should feel the weight of the loss. A player who dies in PvP should feel nothing but the desire to queue again.

The system optimizes for three outcomes:
1. **Defeated players understand why they lost** (Death Recap)
2. **Consequence matches content stakes** (graduated penalty tiers)
3. **No single death ends a session** (recovery is always possible within the run)

---

## PvP Defeat

PvP is a build showcase (see [[PvP Structure]]). Defeat penalties must never discourage queuing.

| Mode | Penalty | Notes |
|------|---------|-------|
| **Arena (1v1)** | Rating change only | No gem loss, no gold loss, no stability penalty. Rating is the only stake |
| **Skirmish (3v3)** | None | Match result affects team ranking only |
| **Battleground (10v10)** | None | Respawn within the match (10s timer). Contribution tracked regardless of deaths |
| **Faction War (Contested Zones)** | Minor faction reputation penalty (-50 rep) | Applies only to flagged PvP deaths in contested zones. No gem or item loss. See [[World Design#Zone Contested Areas]] |

**Hard rule:** No gem loss, no item loss, no gold loss, no stability penalty in any PvP mode. Ever. PvP defeat costs pride and rating. Nothing else.

---

## PvE Defeat: Normal Dungeons

Normal dungeons (Expedition, Farming, Solo Trials) use a forgiving defeat model. The goal is to keep players in the content, not eject them.

### Individual Death

| Property | Detail |
|----------|--------|
| Respawn location | Dungeon entrance or last checkpoint (whichever is closer) |
| Respawn timer | 5s (1st death), 10s (2nd), 20s (3rd), 30s (4th+). Resets between bosses |
| Dungeon progress | **Preserved.** Cleared rooms stay cleared. Boss HP does not reset if at least one party member is alive |
| Gem stability | **No rollback.** Stability changes from the run persist. Mutations earned mid-fight are kept |
| Buffs | All temporary buffs cleared on death (see [[#Buff & State Persistence]]) |
| Momentum | Resets to 5 on respawn |

### Party Wipe (All Members Defeated)

When all party members reach 0 Integrity simultaneously:

| Option | Effect |
|--------|--------|
| **Checkpoint Reset** (default) | Party respawns at last checkpoint. Current boss resets to full HP. Cleared rooms before the checkpoint remain cleared |
| **Full Dungeon Reset** | Party respawns at dungeon entrance. All rooms reset. All boss HP restored. Requires majority party vote |
| **Abandon Run** | Party exits dungeon. No additional penalty beyond time lost |

Party vote is presented immediately on wipe. 15-second timer; default is Checkpoint Reset if no consensus.

### Solo Trial Defeat

Solo Trials have no checkpoints. Death means the trial ends and must be restarted from the beginning. This is the skill-expression cost — trials are short (10-15 minutes) and designed to be repeated. See [[PvE Structure#Solo Trials]].

---

## PvE Defeat: Mythic Dungeons

Mythic dungeons are prestige content with elevated stakes. See [[PvE Structure#Mythic Dungeons]].

### Limited Respawns

| Property | Detail |
|----------|--------|
| Respawns per player | **3 per run** (shared across the entire dungeon, not per boss) |
| Respawn timer | 10s (flat, does not escalate) |
| Respawn location | Last checkpoint |
| Respawn counter | Visible in party UI at all times. Each player's remaining respawns shown individually |

### Party Wipe

| Event | Consequence |
|-------|------------|
| All members reach 0 Integrity | Party is **ejected** from the Mythic dungeon |
| Re-entry | Must restart from the beginning. All progress lost |
| Gem stability | Stability changes from the run **persist**. Mutations are not rolled back |
| Respawn pool | Fully restored on re-entry (new run) |

### Exhausted Respawns

When a player has 0 remaining respawns and reaches 0 Integrity, they are **spectating** for the remainder of the run. They cannot contribute but can observe and communicate with the party. The remaining players continue with reduced numbers.

---

## High-Risk Content

High-risk content is any content where **gem permanent destruction** (stability reaching 0) is a real possibility. This is already defined in the [[Combat System#Gem Stability and Mutation]] — a gem at 0 stability is Broken, and in high-risk content, broken means permanently destroyed.

### What Qualifies as High-Risk

| Content | High-Risk? | Gem Permadeath Active? |
|---------|-----------|----------------------|
| Normal Dungeons (Expedition, Farming) | No | No. Gems at 0 stability are broken for the fight only and can be repaired |
| Solo Trials | No | No |
| Mythic Dungeons | **Yes** | **Yes.** Gems reaching 0 stability are permanently destroyed |
| Challenge Mode (high-tier modifiers) | **Conditional** | Only when the weekly modifier includes "Unstable Field" or equivalent high-risk tag |
| Rift Zones (Obsidian Wastes) | **Yes** | **Yes.** Ambient stability drain creates real breakage risk. See [[World Design#Obsidian Wastes]] |
| Post-MVP: Raids | **Yes** | **Yes** |

### Warning Systems

Players must never accidentally enter high-risk content without understanding the stakes.

| Warning | Trigger | Format |
|---------|---------|--------|
| **Zone border warning** | Approaching a high-risk zone boundary | Amber text: *"WARNING: Entering high-risk area. Gem permanent destruction is possible."* |
| **Dungeon entry warning** | Queuing for or entering a Mythic dungeon | Full-screen confirmation panel listing all equipped gems, their current stability, and rarity |
| **Confirmation prompt** | After warning display | Explicit "I understand the risk" button. No auto-accept. No timer |
| **Gem stability indicator** | During high-risk combat | Gem stability bars pulse red when below 25. Audio cue at Critical threshold |
| **Party UI indicator** | Mythic dungeon party frame | Each player's lowest gem stability visible to all party members |

### Stability Insurance

The [[Economy Model]] defines stability insurance as a gold sink (100-2,000 gold per dungeon entry, scaling with gem rarity and dungeon risk tier). Insurance works as follows:

| Insurance Tier | Cost Range | Effect |
|----------------|-----------|--------|
| **None** | 0 gold | Full risk. Gems at 0 stability are permanently destroyed |
| **Basic** | 100-500 gold | One gem protected. If it reaches 0 stability, it survives at 1 stability instead of being destroyed. Single use per run |
| **Standard** | 500-1,200 gold | All gems protected once. First gem to reach 0 stability survives at 1. Subsequent breakages are not covered |
| **Premium** | 1,200-2,000 gold | All gems protected. Any gem reaching 0 stability survives at 1. Unlimited activations per run |

Insurance is purchased at the dungeon entrance from [[NPC System#Dungeon Guide Pip|Dungeon Guide Pip]] before entering. Cannot be purchased mid-run.

**Design intent:** Insurance is a meaningful gold sink that scales with risk tolerance. Experienced players running familiar Mythic content may skip insurance. Players pushing new Mythic content with expensive gems will pay. This creates an economy loop tied directly to the highest-stakes gameplay. See [[Economy Model#Sink Rates]].

---

## World Boss Defeat

World bosses are community events (10-50 players). Individual death should not punish participation. See [[PvE Structure#World Bosses]].

| Property | Detail |
|----------|--------|
| Death penalty | **None** |
| Respawn timer | 10s (flat) |
| Respawn location | Edge of the world boss arena |
| Contribution tracking | Recorded up to the point of defeat. Dying does not erase prior contribution |
| Rejoin | Immediate after respawn. Player re-enters the fight |
| Gem stability penalty | **None.** World boss combat does not drain gem stability beyond normal in-combat usage |
| Rewards | Based on total contribution across all lives, not just the final phase |

**Rationale:** World bosses are social hype events. Penalizing death in a 50-player encounter with scaling mechanics would discourage participation. The reward is participation and contribution, not survival.

---

## Death Recap

Every defeat — PvP and PvE — triggers a **Death Recap** screen. This is the primary learning tool for combat improvement.

### Death Recap Contents

| Section | Information Shown |
|---------|-------------------|
| **Killing Blow** | The action, domain, and source that dealt the final damage. In PvE: enemy name and action. In PvP: opponent's action name and domain (not their full build) |
| **Damage Timeline** | Last 5 rounds of damage received, broken down by source and domain type. Bar chart format |
| **Integrity Curve** | Graph showing Integrity over the last 10 rounds. Highlights the inflection point where the fight turned |
| **Gem Pressure** | Which of your gems were under stability pressure. Shows stability trajectory per gem over the fight |
| **Momentum Graph** | Your momentum over the fight. Highlights where momentum swung against you |
| **Suggested Adaptations** | 1-3 system-generated tips based on the defeat pattern (e.g., "Opponent used Entropy 6 times -- consider Synthesis to counter decay" or "Gem stability dropped fastest on Kinetics gem -- consider Overcharge alternatives") |

### Death Recap Rules

- PvP Death Recap shows **only your own data** and the opponent's actions that hit you. It does NOT reveal opponent gem stability, full loadout, or draw history. Information warfare is preserved (see [[Combat System#Information Warfare]])
- PvE Death Recap shows full enemy data (intent, domain, damage numbers). Enemies have no information asymmetry to protect
- Death Recap is **dismissable** — players can skip it and respawn immediately
- Death Recap data is **saved** to a combat history log (last 20 defeats) accessible from the hub. Players can review past defeats for pattern analysis

---

## Buff & State Persistence

What survives death and what does not.

### Cleared on Death

| State | Cleared? | Notes |
|-------|----------|-------|
| Temporary combat buffs | Yes | All round-duration and fight-duration buffs removed |
| Momentum | Yes | Resets to 5 on respawn |
| Construct effects | Yes | All active Synthesis constructs despawn |
| Chain stacks (Tempest) | Yes | Reset to 0 |
| Counter-Energy (Aegis) | Yes | Reset to 0 |
| Conditional action state | Yes | No pending conditionals carry over |
| Overcharge effects | Yes | No pending stability costs carry over (stability loss from Overcharge is applied at Aftermath, which occurs before death resolution) |

### Preserved Through Death

| State | Preserved? | Notes |
|-------|-----------|-------|
| Gem mutations | Yes | Stability state and mutation tier persist. A gem that entered Shifting during the fight remains Shifting after respawn |
| Gem mastery progress | Yes | XP earned toward gem mastery during the run is kept |
| Alignment depth progress | Yes | Any alignment points earned during the run persist |
| Dungeon progress (normal) | Yes | Cleared rooms, collected items, partial boss damage (if party members alive) |
| Faction reputation earned | Yes | Rep from kills/objectives during the run is banked immediately |
| Loot collected | Yes | Items picked up before death remain in inventory |
| Quest progress | Yes | Objectives completed before death (e.g., "kill 3 of X") count toward completion |

---

## Death in Narrative Context

Death is not literal in Stormbound Online. When a player's Integrity reaches 0, they are **defeated** — their gem matrix destabilizes and they cannot maintain combat coherence. The narrative framing is:

- **PvP:** "Your gems lose resonance. You yield the field."
- **PvE (dungeon):** "Your gem matrix fractures. You collapse at the entrance / checkpoint, gems flickering."
- **World Boss:** "The force overwhelms your matrix. You stagger back to the arena edge, stabilizing."

This framing avoids "death" as a concept while maintaining mechanical consequence. It also explains why gems (not the character) bear the cost — your body is fine, your gem configuration is what failed.

---

## Summary Table

| Content | Respawn? | Respawn Limit | Gem Stability Rollback | Gem Permadeath | Gold/Item Loss | Rating/Rep Effect |
|---------|----------|--------------|----------------------|----------------|---------------|------------------|
| Arena PvP | N/A (match ends) | N/A | No | No | No | Rating change |
| Skirmish PvP | N/A (match ends) | N/A | No | No | No | Team ranking |
| Battleground PvP | Yes (10s) | Unlimited | No | No | No | None |
| Faction War PvP | Yes (10s) | Unlimited | No | No | No | -50 faction rep |
| Normal Dungeon | Yes (escalating) | Unlimited | No rollback | No | No | None |
| Solo Trial | No (run ends) | 0 | No rollback | No | No | None |
| Mythic Dungeon | Yes (10s) | 3 per player | No rollback | **Yes** (stability 0 = destroyed) | No | None |
| World Boss | Yes (10s) | Unlimited | No | No | No | None |
| Rift Zones | Yes (10s) | Unlimited | No rollback | **Yes** (ambient drain risk) | No | None |

---

## Related Pages

- [[Combat System]] — Integrity, momentum, gem stability mechanics
- [[PvE Structure]] — Dungeon categories, Mythic rules, boss design
- [[PvP Structure]] — Competitive modes, anti-grief systems
- [[Economy Model]] — Stability insurance, gold sinks
- [[World Design]] — Zone contested areas, Rift Zones
- [[Gem Identity System]] — Gem domains, mutation, resonance
- [[Gameplay Loops]] — Session and meta loop integration
- [[NPC System]] — Dungeon Guide Pip (insurance vendor)
