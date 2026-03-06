---
tags:
  - gdd
  - core
  - identity
  - cosmetics
  - titles
---

# Behavioral Identity

## Design Philosophy

Two players equip the same four gems. Same origin. Same gear. Same alignment. They are not the same player.

One overcharges at Momentum 7, hunts for conditional action reads, and locks in every mutation. The other plays conservative, rides Aegis Counter-Energy, and only commits when the information is certain. Their builds are identical on paper. In practice, they are different *people* — and the game should know it, show it, and reward it.

**Behavioral Identity** is the system that reads how you play, generates a living cosmetic fingerprint from that data, and lets you curate which parts of that fingerprint are visible to the world. It is the primary source of player uniqueness in a game without classes.

---

## Behavioral Dimensions

The system tracks 10 behavioral axes. Each axis is a 0-100 spectrum measured over a **rolling 50-hour play window**. The window ensures your identity reflects recent trends, not ancient history — but can't be gamed in a single session.

### The Ten Axes

| Axis | What It Tracks | Low End (0-30) | Mid (31-70) | High End (71-100) |
|---|---|---|---|---|
| **Aggression** | Offensive vs defensive action ratio | Defensive / Reactive | Balanced | Aggressive / Initiator |
| **Risk Appetite** | Overcharge frequency, desperation action usage, low-stability gem riding | Conservative | Calculated | Reckless |
| **Prediction** | Conditional Slot 3 usage rate and success % | Straightforward | Occasional reader | Pattern hunter |
| **Momentum Control** | Average momentum level, time spent at 7+ vs 3- | Low-momentum fighter | Neutral tide | Momentum dominant |
| **Mutation Affinity** | How often mutations are locked in vs reverted | Purist (always revert) | Selective | Mutant (always lock in) |
| **Resonance Seeking** | Resonance trigger rate relative to loadout potential | Incidental | Moderate | Resonance architect |
| **Adaptability** | Build changes per week, gem domain variety used | Specialist (1-2 builds) | Moderate variety | Shapeshifter (5+ builds/week) |
| **Information Warfare** | How quickly behavior changes after opponent reveals (PvP log reading skill) | Reactive | Attentive | Predatory |
| **Content Breadth** | Distribution across PvE, PvP, crafting, trading, exploration | Focused (1-2 types) | Moderate spread | Omnivore (all types) |
| **Social Signature** | Group play ratio, build sharing frequency, teaching frequency | Solo operator | Casual social | Community pillar |

### Measurement Rules

- **Window:** Rolling 50 hours of active gameplay. Older data fades, newer data weighs more
- **Minimum sample:** A dimension needs 10+ hours of relevant data before it becomes active. Until then, it reads as "Unformed" and generates no cosmetic signals
- **Anti-gaming:** Dimension values use weighted rolling averages. A player can't spike a dimension by playing differently for 2 hours. Meaningful shifts require ~8-10 hours of consistent behavioral change
- **PvE vs PvP weighting:** Some axes weight differently by context. Prediction and Information Warfare weight PvP data 2x. Content Breadth weights all equally

---

## Behavioral Signature

The 10 axes combine into a **Behavioral Signature** — a unique fingerprint that drives all cosmetic generation. The signature is visualized internally as a radar chart (never shown raw to the player) but manifests through five cosmetic layers.

### Signature Archetypes

When 3+ axes cluster in the same zone, the system recognizes a **Signature Archetype** — a named pattern that becomes the player's behavioral title root.

| Archetype | Qualifying Axes (3+ must be High) | Description |
|---|---|---|
| **Predator** | Aggression, Risk Appetite, Momentum Control | Ends fights fast. Doesn't care about stability or safety |
| **Architect** | Prediction, Resonance Seeking, Adaptability | Builds are the weapon. Fights are solved before they start |
| **Sentinel** | Low Aggression, Low Risk, High Momentum Control | Waits. Endures. Wins by attrition. Never overextends |
| **Wildcard** | Risk Appetite, Mutation Affinity, Adaptability | Unpredictable. Rides chaos. Every fight is an experiment |
| **Tactician** | Prediction, Information Warfare, Momentum Control | Reads opponents. Exploits patterns. Information is damage |
| **Resonant** | Resonance Seeking, Mutation Affinity, Attunement (stat) | Lives for the echo. Builds around resonance chains |
| **Vanguard** | Aggression, Social Signature, Content Breadth | Aggressive team player. Leads charges. Carries groups |
| **Phantom** | Information Warfare, Low Social, Adaptability | Appears, adapts, wins, vanishes. No pattern to exploit |
| **Forgemaster** | Content Breadth, Social Signature, Low Aggression | The economy player. Builds communities, not kill counts |
| **Unformed** | No 3+ cluster | Early-stage player or genuinely balanced across all axes |

Players can have **one primary** and **one secondary** archetype. If axes shift enough, archetype changes — but the transition takes ~10 hours of new behavior to fully shift.

---

## Five Cosmetic Layers

Each layer manifests behavioral identity differently. All layers are **independently toggleable** — the player chooses what to show.

### Layer 1: Aura

A persistent ambient visual effect around the character in all contexts (hub, dungeon, PvP).

**Generation:**
- **Color** is derived from your two highest behavioral axes
- **Intensity** scales with how strongly your signature clusters (generalists have subtle auras; specialists have vivid ones)
- **Motion** reflects your risk axis (conservative = steady glow, reckless = flickering/unstable)

| Axis Combination | Aura Color |
|---|---|
| Aggression + Momentum | Crimson-gold, pulsing |
| Prediction + Information Warfare | Deep blue, sharp edges |
| Resonance + Mutation Affinity | Iridescent, shifting hues |
| Risk + Adaptability | Violet, crackling |
| Social + Content Breadth | Warm amber, radiating outward |
| Low-cluster (balanced) | Silver-white, soft |

**Aura Tiers:**
- **Forming** (0-20 hours tracked): Faint shimmer, barely visible
- **Defined** (20-50 hours): Clear color and motion, recognizable
- **Saturated** (50-100 hours): Vivid, distinctive. Other players can identify your archetype at a glance
- **Transcendent** (200+ hours with consistent signature): Aura interacts with environment text — hub descriptions mention your presence

**Player control:** Toggle on/off. Cannot change color (it reflects you). Can set to "Muted" for PvP anonymity (visible only to self and party).

---

### Layer 2: Combat Log Voice

How the combat log narrates YOUR actions. This is the most personal cosmetic layer — it changes the *words* the game uses to describe what you do.

**Base combat log:**
```
[R4] You commit: Storm Strike → Corrode → Consume
```

**Behavioral combat log variants:**

| Archetype | Same Action, Different Voice |
|---|---|
| Predator | `[R4] You lunge: Storm Strike tears through → Corrode eats what remains → Consume finishes the meal` |
| Sentinel | `[R4] You respond: Storm Strike answers → Corrode follows, measured → Consume closes the exchange` |
| Wildcard | `[R4] You gamble: Storm Strike — why not? → Corrode unravels something → Consume takes whatever's left` |
| Tactician | `[R4] You execute: Storm Strike (predicted opening) → Corrode (targeting revealed weakness) → Consume (calculated extraction)` |
| Architect | `[R4] You sequence: Storm Strike (chain 3→4) → Corrode (debuff stacks to 2) → Consume (resonance window open)` |
| Phantom | `[R4] You strike: Storm Strike, Corrode, Consume — nothing wasted` |

**Voice evolution:**
- Voice shifts gradually as behavioral signature changes. Players notice their log "talking differently" over time
- At Transcendent aura tier, the combat log develops **micro-personality** — occasional commentary on particularly good or bad rounds:
  - Predator: `"That wasn't close."`
  - Sentinel: `"Patience rewarded."`
  - Wildcard: `"Even you didn't see that coming."`

**Player control:** Choose from unlocked voice styles. Default is your current archetype voice. Can equip any archetype voice you've held for 20+ hours. Can disable behavioral voice entirely (reverts to plain log).

---

### Layer 3: Hit Effects

How damage numbers and combat effects are styled when YOU deal or receive them.

| Archetype | Crit Text Style | Normal Hit Style | Whiff Style |
|---|---|---|---|
| Predator | `>>> 612 <<<` (bold, sharp) | `342` (clean) | `—miss—` (curt) |
| Sentinel | `[612]` (contained, solid) | `342` (steady) | `(absorbed)` |
| Wildcard | `~*612*~` (unstable, sparking) | `342?` (uncertain) | `¿?¿` |
| Tactician | `612 [predicted]` | `342 [on target]` | `0 [reassessing]` |
| Architect | `612 {resonance}` | `342 {sequence}` | `— {recalibrating}` |

**Evolution:**
- At Saturated aura tier, your crit effects gain a secondary flourish — a brief text animation specific to your highest behavioral axis
- Momentum-dominant players' crits display their current momentum: `612 [M:9]`
- Resonance seekers' crits show active resonance count: `612 {R:3}`

**Player control:** Toggle on/off. Cannot change style (reflects behavior). Can disable for "clean" combat log (numbers only).

---

### Layer 4: Titles

Titles are the most visible identity element. They combine **behavioral archetype**, **milestone achievements**, and **player-chosen suffixes** into a layered naming system.

#### Title Anatomy (expanded from [[Cosmetic System]])

```
[Radiant] ★★★ Tactician Stormweaver Kael, Resonance Explorer
   ↑        ↑      ↑          ↑       ↑          ↑
alignment mastery behavioral  gem     name     milestone
 badge    stars  archetype  archetype          suffix
```

#### Behavioral Archetype Title (Dynamic)

- Generated from your Signature Archetype (see above)
- Updates when your archetype shifts (with ~10 hour buffer to prevent flickering)
- Displayed before gem archetype title if player enables it
- Players who have held an archetype for 100+ cumulative hours earn the **Proven** modifier: "Proven Tactician" instead of "Tactician"
- Players at 500+ cumulative hours earn the **Legendary** modifier: "Legendary Predator"

#### Milestone Suffixes (Permanent)

Earned through specific achievements. Once earned, permanently available. Player chooses which to display.

| Milestone | Suffix | Requirement |
|---|---|---|
| First Blood | "the Blooded" | Win first PvP match |
| Resonance Explorer | "Resonance Explorer" | Discover 20 unique resonances |
| Momentum Master | "of the Unstoppable Tide" | Reach Momentum 10 in 50 separate fights |
| Mutation Embracer | "the Mutated" | Lock in 100 gem mutations |
| Architect of Ruin | "Architect of Ruin" | Break 50 enemy gems (reduce to 0 stability) in PvP |
| The Patient | "the Patient" | Win 25 PvP fights lasting 15+ rounds |
| Unbreakable | "the Unbreakable" | Win 10 fights after being at Desperation momentum |
| Omnivore | "of All Paths" | Reach Trusted reputation with all 6 factions |
| Gemwright | "Gemwright" | Craft 25 Epic+ gems |
| Legend Maker | "Legend Maker" | Have a published build used by 100+ players |
| The Unseen | "the Unseen" | Win 20 PvP fights where opponent never correctly predicted your conditional action |
| Void Walker | "of the Void" | Spend 50+ cumulative rounds at Critical gem stability (25-1) and win |
| Convergence | "of Convergence" | Trigger all 6 possible resonance pairs in a single fight |

#### Title Display Rules

- Maximum displayed: 1 behavioral title + 1 milestone suffix
- Behavioral title can be hidden (gem archetype alone is shown)
- Milestone suffix can be swapped at any time
- Titles are never purchasable. Money cannot buy behavioral identity

---

### Layer 5: Build Card Evolution

The build card (see [[Cosmetic System]]) evolves beyond a static snapshot into a **living document** of the player's behavioral history.

#### Standard Build Card (Level 1-49)

Same as current [[Cosmetic System]] design — gem loadout, stats, build code.

#### Evolved Build Card (Level 50+)

Unlocks additional sections driven by behavioral data:

```
╔══════════════════════════════════════════╗
║  [Radiant] ★★★ Tactician Stormweaver    ║
║  Kael, Resonance Explorer               ║
║  Ironsworn · Lv 50 (P53)               ║
╠══════════════════════════════════════════╣
║  BEHAVIORAL SIGNATURE                    ║
║  ▓▓▓▓▓▓▓░░░  Prediction (74)           ║
║  ▓▓▓▓▓▓░░░░  Information Warfare (63)  ║
║  ▓▓▓▓▓▓░░░░  Momentum Control (61)     ║
║  ▓▓▓▓▓░░░░░  Resonance Seeking (55)    ║
║  ▓▓▓░░░░░░░  Risk Appetite (32)        ║
║  ─── Primary: Tactician ───             ║
║  ─── Secondary: Architect ───           ║
╠══════════════════════════════════════════╣
║  GEM LOADOUT                             ║
║  ◈ Tempest     Stability Pref: Stable   ║
║  ◈ Entropy     Mutations Locked: 12     ║
║  ◈ Resonance   Mastery: 8 ████████░░   ║
║  ◈ Void        Mastery: 6 ██████░░░░   ║
╠══════════════════════════════════════════╣
║  COMBAT TENDENCY                         ║
║  Opener: Prediction-based (varies)      ║
║  Mid-fight: Resonance chaining          ║
║  Finisher: Void Consume on debuffed     ║
║  Momentum Profile: ░░░▓▓▓▓▓▓░ (avg 6.8)║
╠══════════════════════════════════════════╣
║  STATS                                   ║
║  FRC:132 PRC:128 RES:113                ║
║  ACU:141 ATT:131 INS:122               ║
╠══════════════════════════════════════════╣
║  BUILD CODE: SB-IRN-RAD-Tk4v9          ║
║  Behavioral Hash: #7CF2A (unique)       ║
╚══════════════════════════════════════════╝
```

**Behavioral Hash:** A short alphanumeric code generated from your full 10-axis signature. No two players with different behavioral patterns share a hash. It's a fingerprint — proof of uniqueness. Players can compare hashes the way they compare build codes.

#### Build Card Visibility Controls

| Section | Default | Can Hide? |
|---|---|---|
| Gem Loadout | Visible | No (core identity) |
| Stats | Visible | Yes |
| Behavioral Signature | Visible | Yes (can show/hide individual axes) |
| Combat Tendency | Hidden | Yes (opt-in reveal) |
| Behavioral Hash | Visible | Yes |

---

## PvP Anonymity System

Because cosmetic signals leak information, PvP contexts offer masking controls.

### Masking Modes

| Mode | What's Hidden | When Available |
|---|---|---|
| **Open** (default) | Nothing hidden | Always |
| **Veiled** | Aura muted, behavioral axes hidden on build card, hit effects revert to plain | Available at any time |
| **Shrouded** | All of Veiled + behavioral title hidden, combat log voice reverts to default | Unlockable at "The Unseen" milestone |
| **Anonymous** | All of Shrouded + gem archetype replaced with "Unknown" | Tournament mode only (admin-controlled) |

**Rule:** Masking is a PvP *choice*, not a default. The information warfare design is that cosmetic signals are valuable intelligence — choosing to hide them is itself a signal (opponent knows you're hiding something).

### Reading Opponent Signals

When facing an opponent in PvP, visible cosmetic layers provide inference material:

| Signal | What You Can Infer |
|---|---|
| Crimson-gold pulsing aura | High Aggression + Momentum Control — expect aggressive openers and chain pressure |
| Deep blue sharp aura | Prediction + Information — expect conditional actions and pattern adaptation |
| "Proven Tactician" title | 100+ hours of prediction-heavy play — this player reads combat logs |
| "the Mutated" suffix | Heavy mutation user — their gems may behave unexpectedly |
| Iridescent aura | Resonance seeker — watch for resonance combo setups |
| Muted aura | **Deliberately hidden** — could be anything. Treat as wildcard |

This makes the pre-fight inspection a genuine strategic moment in PvP.

---

## PvE Behavioral Effects

> Titles are no longer purely cosmetic in PvE. Your behavioral identity feeds back into how the game world responds to you.

### Design Guardrails

Before defining effects, the constraints:

1. **No playstyle produces strictly better loot.** Different behaviors unlock *different* rewards, never *better* ones
2. **Difficulty adjustments are always paired with reward adjustments.** Harder content from behavioral scaling gives proportionally more rewards
3. **Effects are 5-15% modifiers**, not fundamental changes. A player unaware of this system still clears all content normally
4. **Anti-gaming:** Uses the rolling 50-hour window. Cannot be manipulated in a single session
5. **Transparency:** Players can inspect their active PvE modifiers in the build panel. Nothing is hidden

### Dungeon AI Adaptation

Enemy behavior subtly shifts based on the party's collective behavioral signature.

| Party Behavioral Lean | AI Response | Design Intent |
|---|---|---|
| High Aggression | Enemies use defensive abilities more frequently (+15%). Boss adds spawn slightly earlier | Punishes mindless aggression, rewards tactical burst timing |
| High Defense / Low Risk | Enemies become more aggressive (+10% attack frequency). Enrage timers tighten by 5% | Prevents infinite stall strategies, rewards calculated offense |
| High Prediction | Boss telegraph patterns become less predictable (rotate between 2-3 variants instead of fixed sequence) | Rewards deep log reading, creates richer prediction puzzle |
| High Resonance Seeking | Dungeon gem ecology becomes more volatile (more frequent environmental resonance triggers, both beneficial and dangerous) | Creates more resonance opportunities AND hazards |
| High Mutation Affinity | Gem stability drain increases by 10% in dungeons, but mutation outcomes are biased toward beneficial variants | Feeds the mutation player's appetite while increasing management pressure |
| Balanced / No Strong Lean | Standard dungeon tuning | Default experience |

**Party blending:** In groups, the AI reads the party's averaged behavioral signature. A party of 4 aggressive players faces strongly defensive AI. A mixed party gets a blended response. This creates emergent party composition strategy beyond gem domains alone.

### Resonance and Mutation Influence

| Behavioral Condition | Effect |
|---|---|
| Resonance Seeking > 60 | +5% resonance trigger chance in dungeons (stacks with Attunement stat bonus) |
| Mutation Affinity > 60 | Mutation outcomes when locking in have +10% chance of the "favorable" branch |
| Prediction > 70 | Dungeon boss "tells" include one additional hint in the telegraph text |
| Information Warfare > 70 | Enhanced combat log in PvE — shows enemy gem stability numbers (normally hidden) |

### Title-Gated Content

Specific behavioral milestones unlock hidden dungeon content. This is the most tangible reward for developing a behavioral identity.

| Requirement | Unlocks | Location |
|---|---|---|
| "Mutation Embracer" milestone (100 locked mutations) | **Mutation Vault** — hidden room with a chest that drops gems with pre-applied rare mutations | Any dungeon with Entropy gem ecology |
| Resonance Seeking > 70 for 20+ hours | **Harmonic Corridor** — secret path where every step triggers a resonance event (high reward, high chaos) | Stormspire dungeon |
| Prediction > 70 + "The Patient" milestone | **Oracle's Trial** — solo encounter where the boss has no telegraph. Win by pure combat log reading | Celestial Peaks (Post-MVP) |
| "Void Walker" milestone (50 rounds at Critical stability) | **The Abyss** — a hidden floor where all gems start at 50 stability. Unique cosmetics and a void-themed gem variant drop | Void Nexus (Post-MVP) |
| Content Breadth > 80 for 50+ hours | **The Convergence Hub** — a hidden social space accessible only to omnivore players, with a unique NPC vendor selling cross-faction recipes | Sky Forum, Celestial Peaks (Post-MVP) |

### Dynamic Dungeon Difficulty Scaling

When entering a dungeon, the system calculates a **Behavioral Difficulty Modifier (BDM)** from the party's collective signature.

```
BDM = 1.0 + (Strongest_Axis_Average - 50) * 0.002
```

| Party's Strongest Axis Average | BDM | Effect |
|---|---|---|
| 30 (low specialization) | 0.96 | -4% enemy HP/damage. Slightly easier |
| 50 (average) | 1.00 | Standard tuning |
| 70 (high specialization) | 1.04 | +4% enemy HP/damage. Slightly harder |
| 90 (extreme specialization) | 1.08 | +8% enemy HP/damage. Noticeably harder |

**Reward scaling mirrors BDM exactly:** BDM 1.08 = +8% gem drop quantity and +8% gold. Difficulty always pays for itself.

**Mythic modifier interaction:** BDM stacks with weekly Mythic modifiers. A party of extreme specialists in a Mythic dungeon faces the highest difficulty — and earns the highest rewards.

---

## Behavioral Loot Affinity

Your behavioral signature subtly influences *what type* of rewards you receive, not *how much*.

| Dominant Axis | Loot Bias | Reasoning |
|---|---|---|
| Aggression | Offensive gem domains drop +10% more frequently | Feed the aggressive player's build path |
| Risk Appetite | Higher rarity variance (more commons AND more rares, fewer uncommons) | Match the risk player's all-or-nothing mindset |
| Prediction | Modifier gems that enable conditional bonuses drop +10% | Tools for the prediction player's toolkit |
| Resonance Seeking | Gems with higher resonance potential drop +10% | Feed the resonance builder's appetite |
| Mutation Affinity | Gems drop with lower starting stability (+chance of pre-mutation) | Give the mutation rider something to work with |
| Social Signature | Materials and tradeable goods drop +10%, personal-use gems -5% | Feed the community player's economic role |
| Content Breadth | Loot pool draws from all dungeon families, not just the current one | Reward the omnivore with variety |

**No bias exceeds 10%.** The overall loot quantity remains the same — only the distribution within a drop table shifts.

---

## Cosmetic Economy

### Earned Cosmetics (Behavioral)

| Cosmetic | Source | Tradeable |
|---|---|---|
| Aura color/intensity | Behavioral signature (automatic) | No |
| Aura tier (Forming → Transcendent) | Play hours with consistent signature | No |
| Combat log voice | Behavioral archetype (automatic) | No |
| Hit effect style | Behavioral archetype (automatic) | No |
| Behavioral title prefix | Behavioral archetype (automatic) | No |
| Milestone suffix | Achievement-based | No |
| Build card evolution | Level 50+ behavioral data | No |
| Title modifiers (Proven, Legendary) | Cumulative archetype time | No |

### Earned Cosmetics (Achievement)

| Cosmetic | Source | Tradeable |
|---|---|---|
| Name color | Gem mastery rarity (see [[Cosmetic System]]) | No |
| Mastery stars | Gem mastery achievement count | No |
| Alignment badge | Alignment depth (see [[Alignment System]]) | No |
| Faction badge | Faction reputation tier (see [[Factions System]]) | No |

### Purchasable Cosmetics (Cash Shop)

| Cosmetic | Price Range | Rules |
|---|---|---|
| Title suffixes (flavor only) | $3-5 | Cannot mimic milestone suffixes. Clearly marked as purchased |
| Chat flair sets | $5-8 | Border/color on chat messages |
| Profile banners | $5-8 | Profile page header art |
| Build card skins | $3-5 | Visual frame only — does not affect behavioral data display |
| Emote packs | $5 | Custom emote text in chat |
| Battle pass (seasonal) | $10/season | Cosmetic track only. Includes seasonal aura filter (tint overlay, does not replace behavioral color) |

### The Cardinal Rule

> **Behavioral cosmetics cannot be purchased.** Your aura, your combat log voice, your hit effects, your behavioral title, your milestone suffixes — these are YOU. They are earned by playing, not by paying. The cash shop sells *decoration*. The behavioral system sells *nothing*.

---

## Seasonal Behavioral Snapshots

At the end of each competitive season (quarterly), the system takes a **Behavioral Snapshot** — a frozen record of your signature at that moment.

- Snapshots are displayed on your profile as historical records
- Each snapshot includes: archetype, top 3 axes, aura color, milestone count, behavioral hash
- Players accumulate a **behavioral timeline** — visible history of how their playstyle evolved over months/years
- Seasonal snapshot earns a unique badge: "Season [X] — [Archetype]"
- First-season-ever snapshot for any archetype earns the "Pioneer" badge for that archetype

This creates long-term investment in identity. Your behavioral history becomes part of your legacy.

---

## Implementation Priority

| Feature | Priority | MVP? |
|---|---|---|
| 10 behavioral axes tracking | Critical | Yes (backend, no UI yet) |
| Signature Archetype detection | Critical | Yes |
| Behavioral title prefix | High | Yes |
| Milestone suffix system | High | Yes (5-8 milestones at launch) |
| Combat log voice variants | High | Yes (3 archetypes at launch, expand post-MVP) |
| Aura system | Medium | Post-MVP (requires visual framework) |
| Hit effect styling | Medium | Yes (text-based, low implementation cost) |
| Evolved build card | Medium | Post-MVP |
| PvE AI adaptation | Medium | Post-MVP (start with basic aggression/defense response) |
| Title-gated content | Low | Post-MVP |
| Behavioral loot affinity | Low | Post-MVP |
| Dynamic difficulty scaling (BDM) | Low | Post-MVP |
| Seasonal snapshots | Low | Post-MVP |
| PvP anonymity modes | Medium | Yes (Open + Veiled at launch) |

---

**Related:** [[Cosmetic System]] | [[Gem Identity System]] | [[Combat System]] | [[Progression Systems]] | [[Factions System]] | [[UI Design]] | [[PvP Structure]] | [[PvE Structure]]