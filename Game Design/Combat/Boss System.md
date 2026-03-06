---
tags:
  - gdd
  - combat
  - boss
  - pve
---

# Boss System

## Design Philosophy

Bosses are the game's signature content. Every boss is a hand-authored tactical puzzle that operates within the same [[Combat System|phase-based combat]] rules as players. No boss cheats the system — they Draw, Commit, Clash, and suffer Aftermath like everyone else. The difference is that bosses have unique gem loadouts, exclusive actions, proprietary resonances, and multi-phase AI that transforms the fight as it progresses.

A boss encounter should feel like a conversation: the boss speaks through intent telegraphs and action patterns, the player responds through commits and conditionals, and the combat log is the transcript that reveals who understood whom.

### Core Principles

From [[PvE Structure#Boss Design Philosophy]]:

1. **Telegraph attacks** — every major attack has a readable windup. Punishment is for failing to respond, not failing to guess
2. **Reward positioning** — displacement, zone control, and spatial awareness matter against bosses
3. **Have phases** — HP thresholds trigger new mechanics. Each phase tests a different combat skill
4. **Punish greed** — overcommitting during a boss telegraph should result in taking the hit
5. **Enable counterplay** — at least one boss attack per encounter should be counterable for a stagger window
6. **Never be HP sponges** — if the fight is long, it's because mechanics demand careful play
7. **Never require specific domains** — any loadout should be able to clear, with efficiency varying by synergy

---

## Boss AI Framework

Bosses use the same phase structure as players but with a deterministic AI layer that creates readable, learnable patterns.

### How Boss Turns Work

| Phase | Player | Boss |
|-------|--------|------|
| **Draw** | Random from gem pool | Determined by AI state + phase rules |
| **Commit** | Player choice | AI selects from priority tables (see below) |
| **Clash** | Simultaneous resolution | Same rules as PvP |
| **Aftermath** | Same | Boss mutations, construct ticks, phase transitions |

Bosses do NOT draw randomly. Their action selection follows **priority tables** — ordered lists of conditions and responses. This makes bosses learnable without being trivially predictable.

### Priority Table Structure

Each boss has a priority table per phase. The AI evaluates conditions top-to-bottom and executes the first matching entry.

```
BOSS AI: Storm Leviathan (Phase 1)
──────────────────────────────────
Priority 1: IF player momentum ≥ 9
            → Commit: Tidal Surge (displacement) + Leviathan Guard (defense)
            Reason: Punish dominance, reset tempo

Priority 2: IF boss Chain stacks ≥ 4
            → Commit: Lightning Barrage (burst spend) + Tail Sweep (AoE)
            Reason: Cash in chain investment

Priority 3: IF any player gem stability < 30
            → Commit: Focused Storm (target weakest gem) + Storm Strike (chain build)
            Reason: Snipe vulnerable gems

Priority 4: IF round count % 3 == 0
            → Commit: Charged Ground (zone hazard) + Storm Strike (chain build)
            Reason: Periodic pressure, zone control

Priority 5: DEFAULT
            → Commit: Storm Strike (chain build) + Lightning Chain (damage)
            Reason: Standard offense, build toward burst
──────────────────────────────────
```

**Key properties:**
- Priority tables are deterministic but context-dependent — the boss's action changes based on game state
- Players who read the [[Combat Logs System|combat log]] can identify which priority fired and predict future behavior
- Phase transitions swap the entire priority table, creating a "new boss" feeling
- Mythic bosses have deeper tables with more conditional branches

### Boss Intent Display

In PvE, bosses telegraph partial intent during the Draw phase (see [[Combat System#PvE Combat]]):

```
┌─────────────────────────────────────────┐
│  STORM LEVIATHAN           HP ████████░░│
│  ─────────────────────────────────────  │
│  INTENT: [Offensive] → [Offensive]      │
│  DOMAIN: Tempest (revealed)             │
│  STABILITY: ████████░░ ~80%             │
│  HINT: High chain stacks. Expect burst. │
└─────────────────────────────────────────┘
```

| Information | What's Shown | What's Hidden |
|-------------|-------------|---------------|
| Intent category | Offensive / Defensive / Utility per slot | Specific action names |
| Domain | Revealed after first use (permanent) | Unequipped domains |
| Stability | Approximate bar (±10%) | Exact number |
| Hints | Tactical guidance based on boss state | Priority table logic |
| Conditional slot | "Boss is preparing something..." | The trigger condition |

**Behavioral Identity bonus:** Players with Prediction > 70 (see [[Behavioral Identity]]) get one additional hint line in the telegraph text, revealing more about the boss's conditional logic.

---

## Boss Gem Ecology

Bosses have their own gem loadouts following the same domain rules as players. This is not cosmetic — boss gems mutate, resonate, and break.

### Boss Gem Rules

| Property | Rule |
|----------|------|
| Gem count | 2-4 gems depending on boss tier |
| Domains | Match the dungeon's domain ecology (see [[World Design#Gem Domain Ecology]]) |
| Starting stability | 100 (same as players) |
| Mutation | Same thresholds (Stable → Shifting → Unstable → Critical → Broken) |
| Resonance | Bosses have unique proprietary resonances not available to players |
| Visibility | Stability shown as approximate bar. Domains revealed on first use |

### Breaking Boss Gems

This is the strategic depth layer that separates good parties from great ones.

| Event | Effect |
|-------|--------|
| Boss gem enters Shifting (75) | One boss action replaced by a weaker variant |
| Boss gem enters Unstable (50) | Boss loses access to one action entirely. AI priority table adapts |
| Boss gem enters Critical (25) | Boss loses an entire priority branch. Becomes more predictable |
| Boss gem Breaks (0) | Domain removed from boss loadout. All actions from that domain gone. AI falls back to remaining gems |

**Breaking a boss gem changes the fight fundamentally.** A Stormspire boss that loses its Tempest gem can no longer chain-build. A Void Nexus boss that loses Entropy stops applying debuffs. Parties can choose between:

- **Damage race** — ignore boss stability, burn Integrity directly
- **Gem attrition** — target boss gems with Entropy (Destabilize), Void (stability steal), or Overcharge-heavy pressure to break gems and simplify the fight
- **Hybrid** — weaken one gem to remove the most dangerous action, then damage race the rest

### Boss Resonances

Boss resonances are **unique** — they don't exist in the player resonance pool.

| Property | Detail |
|----------|--------|
| Discovery | Not documented. Players learn them through experience and combat log reading |
| Triggering | Bosses try to trigger their resonances via priority table conditions |
| Prevention | Players can disrupt boss resonances by breaking the contributing gem or denying the action sequence |
| Signaling | [[Combat Logs System#Resonance Signals]] apply — the log hints when a boss resonance is building |

Example boss resonance:

```
BOSS RESONANCE: Storm Leviathan — "Eye of the Maelstrom"
  Trigger: Tempest Chain stacks ≥ 5 + Kinetics displacement in same round
  Effect: Arena becomes fully electrified for 2 rounds. ALL positions deal
          Tempest damage. Only safe position: directly adjacent to the boss.
  Prevention: Break boss Tempest gem below 5 stacks, or deny Kinetics
              displacement with counter-positioning.
```

---

## Boss Tiers

### Mini-Boss

Found in farming dungeons and as mid-dungeon encounters in Expeditions.

| Property | Detail |
|----------|--------|
| Gems | 2 |
| Phases | 1 (no phase transitions) |
| Rounds | 5-10 |
| Duration | 1-3 min |
| Intent | Full category shown + basic hints |
| Resonances | None (too few gems) |
| Priority table | 3-4 entries, simple conditions |

Mini-bosses teach the phase-based boss combat vocabulary without overwhelming. They telegraph clearly, have limited action pools, and punish exactly one mistake type per encounter.

**Design template:**
- 1 signature attack (telegraphed, avoidable)
- 1 standard rotation
- 1 punish condition (e.g., "if player overcommits, counterattack")

### Expedition Boss

The core boss experience. 2-3 per Expedition dungeon.

| Property | Detail |
|----------|--------|
| Gems | 3 |
| Phases | 2-3 (HP threshold triggers) |
| Rounds | 15-25 |
| Duration | 5-8 min |
| Intent | Category shown, hints available |
| Resonances | 1-2 unique boss resonances |
| Priority table | 5-8 entries per phase, context-dependent |

Each phase introduces a new mechanic while retaining core patterns. Phase transitions are narrated in the combat log:

```
AFTERMATH ─────────────────────────
  !! PHASE TRANSITION: Storm Leviathan enters Phase 2 — Charged Ground
     ↳ Arena floor becomes electrified in zones
     ↳ New mechanic: Safe positions shift each round
     ↳ Boss gains: Leviathan Charge (high-damage displacement)
──────────────────────────────────
```

**Design template per phase:**
- Phase 1: Teach the boss's core pattern. Fair, readable, introductory
- Phase 2: Introduce a spatial/environmental mechanic on top of the core pattern
- Phase 3: All previous mechanics active simultaneously + enrage soft timer

### Mythic Boss

Rare, rotating, prestige encounters. The hardest PvE content.

| Property | Detail |
|----------|--------|
| Gems | 4 (full player equivalent) |
| Phases | 3-4 |
| Rounds | 25-40 |
| Duration | 8-12 min |
| Intent | Category only, fewer hints. Behavioral Identity bonuses matter here |
| Resonances | 2-3 unique boss resonances, some conditional on player actions |
| Priority table | 8-12 entries per phase, deep conditional logic |

Mythic bosses are where gem attrition strategy becomes critical. A 4-gem boss with 3 resonances presents a puzzle: which gem do you target first? Which resonance is the most dangerous? Can you prevent the boss from triggering its conditional resonance while maintaining your own offense?

**Mythic-specific mechanics:**
- **Reactive resonances** — triggered by player actions, not just boss state. ("If players trigger a resonance, the boss answers with its own")
- **Gem recovery** — Mythic bosses can stabilize their own gems (1 per fight, predictable timing). Parties must time gem attrition around this
- **Integrity gates** — at certain HP thresholds, the boss becomes invulnerable until a specific mechanic is resolved (interrupt a channel, destroy a construct, break a specific gem)
- **Combat log complexity** — less information freely given. Enhanced log at high momentum becomes genuinely valuable

### World Boss

Open-world encounters for 10-50 players. Different design paradigm.

| Property | Detail |
|----------|--------|
| Gems | 4-6 (scaled beyond player limits) |
| Phases | 3-5 |
| Rounds | 30-60 |
| Duration | 10-20 min |
| Scaling | HP, damage, and action count scale with participant count |
| Intent | Broad telegraphs visible to all. Individual targeting shown per player |
| Resonances | 1-2 spectacle resonances (big, visible, exciting for large groups) |

World bosses prioritize **spectacle and accessibility** over precision tactics. They're community events, not optimization puzzles.

**Scaling rules:**
- Base: Tuned for 10 players
- Per additional player: +8% HP, +3% damage, +1 additional target per AoE action (cap at 5)
- At 30+ players: Boss gains 1 additional gem (5 total) with a new action set
- At 50 players: Boss gains full 6-gem loadout, enabling unique world-boss-only resonances

**Contribution system:**
- Loot eligibility requires meaningful contribution (damage dealt, damage absorbed, resonances triggered, mechanics handled)
- Contribution is percentage-based — a level 20 player contributing their best effort qualifies alongside level 50 veterans
- World Boss Herald NPC (see [[NPC System]]) announces spawns 30 minutes in advance
- See [[State Persistence & Recovery]] for disconnect handling during world boss encounters

---

## MVP Boss Encounters

5 Expedition dungeons × 2-3 bosses each = **13 bosses at MVP**. Plus 1 world boss and 2-3 mini-bosses per dungeon.

### Tidecaller Caverns (Calm Shores)

**Domain ecology:** Aegis / Synthesis

#### Boss 1: Tidecaller Sentinel — Mini-Boss

| Property | Detail |
|----------|--------|
| Gems | Aegis, Synthesis |
| Phases | 1 |
| Rounds | 5-8 |
| Role | Tutorial boss. Teaches intent reading and commit timing |

**Priority Table:**
1. IF player used Offensive in both slots last round → Fortify + Reflect (punish aggression)
2. IF Construct active for 2+ rounds → Catalyze (burst from constructs)
3. DEFAULT → Construct + Fortify (build and defend)

**Lesson:** Don't blindly attack into a shielding enemy. Read the intent, wait for the defensive round, then commit offense when the boss is building constructs.

#### Boss 2: The Tidecaller — Expedition Boss

| Property | Detail |
|----------|--------|
| Gems | Aegis, Synthesis, Resonance |
| Phases | 3 |
| Rounds | 15-20 |
| Unique Resonance | "Tidal Echo" — Aegis + Resonance: all Counter-Energy stored by the boss also generates an echo that heals the boss 1 round later |

**Phase 1 (100-60%): The Rising Tide**
- Boss builds Counter-Energy through Fortify and converts it via Reflect
- Constructs place healing zones that restore boss Integrity over time
- **Lesson:** Destroy healing constructs quickly. Don't feed Counter-Energy with predictable attacks

**Phase 2 (60-30%): Riptide**
- Boss gains Tidal Pull — a Kinetics-style displacement that yanks players into construct zones
- Echo mechanic active: every boss defensive action echoes as a heal next round
- **Lesson:** Break the Resonance gem to disable Tidal Echo. Or break Synthesis to stop healing constructs. Choose your attrition target

**Phase 3 (30-0%): Maelstrom**
- All mechanics active. Boss commits 3 actions per round instead of 2
- Enrage: healing constructs become damage constructs after 5 rounds
- **Lesson:** DPS check combined with mechanic execution. Time your burst for when the boss commits to Construct (defensive round)

---

### Stormspire (Stormreach)

**Domain ecology:** Tempest / Kinetics

#### Boss 1: Stormclaw Alpha — Mini-Boss

| Property | Detail |
|----------|--------|
| Gems | Tempest, Kinetics |
| Phases | 1 |
| Rounds | 6-10 |
| Role | Teaches chain mechanics and displacement |

**Priority Table:**
1. IF Chain stacks ≥ 4 → Surge (spend stacks for burst) + Force Strike (push)
2. IF player is in adjacent position → Storm Strike (chain build) + Gravity Well (pull back)
3. DEFAULT → Storm Strike + Lightning Chain

**Lesson:** The boss builds chains and spends them for burst. Track chain stacks in the combat log. When stacks are high, commit defensive or displace away from burst range.

#### Boss 2: Thunder Warden — Expedition Boss

| Property | Detail |
|----------|--------|
| Gems | Tempest, Kinetics, Aegis |
| Phases | 2 |
| Rounds | 12-18 |
| Unique Resonance | "Grounded Fury" — Kinetics + Aegis: if the boss displaces a player AND blocks in the same round, Counter-Energy is doubled |

**Phase 1 (100-50%): Warden's Guard**
- Boss alternates between chain-building rounds and defensive rounds
- Displacement used to control positioning — pulls players into hazard zones
- **Lesson:** Read the alternation pattern. Attack during chain rounds, defend during guard rounds

**Phase 2 (50-0%): Unleashed**
- Pattern breaks — boss chains become unpredictable. Priority table gains 3 new conditional entries
- Grounded Fury resonance becomes active. Boss actively tries to displace + block in the same round
- Arena gains 2 electrified zones that deal Tempest damage (shift position each round)
- **Lesson:** Prevent the resonance by denying displacement (counter-position with your own Kinetics) or break the Aegis gem to remove the block component

#### Boss 3: Storm Leviathan — Expedition Boss (Signature)

| Property | Detail |
|----------|--------|
| Gems | Tempest, Kinetics, Entropy |
| Phases | 3 |
| Rounds | 20-25 |
| Unique Resonances | "Eye of the Maelstrom" (Tempest + Kinetics), "Corrosive Storm" (Tempest + Entropy) |

**Phase 1 (100-60%): Thunder Strikes**
- Lightning pillars target random players (2-round telegraph in combat log)
- Tail Sweep is counterable — successful counter staggers boss for 1 round (free damage window)
- Chain Lightning targets players who commit identical actions (punishes copycat sequences)
- **Lesson:** Vary your commits. Watch for Tail Sweep telegraph. Counter it for a stagger window

**Phase 2 (60-30%): Charged Ground**
- Arena floor becomes electrified in zones — safe positions shift each round based on boss displacement actions
- Leviathan Charge: massive displacement + damage (3-round telegraph, dodge by committing a repositioning action)
- Lightning Rods spawn — standing near a rod absorbs incoming Tempest damage but the rod breaks after 2 absorptions
- **Lesson:** Positioning puzzle. Track safe zones through the combat log. Use Lightning Rods strategically (save them for burst rounds, not chip damage)

**Phase 3 (30-0%): Storm Convergence**
- All Phase 1 + Phase 2 mechanics active simultaneously
- Interruptible Channel: boss charges for 3 rounds. If not interrupted (requires breaking a specific gem below 50 stability during the channel), arena-wide damage that kills most players
- Enrage soft timer: storm intensity increases every 5 rounds, shrinking safe ground by 1 position each time
- Eye of the Maelstrom resonance becomes active. If triggered, ALL positions become dangerous for 2 rounds — only adjacent to boss is safe
- **Lesson:** Manage gem attrition AND positioning AND DPS simultaneously. The interrupt check forces gem-targeting strategy. The resonance forces proximity risk

---

### Thornwood Hollow (Verdant Depths)

**Domain ecology:** Synthesis / Resonance

#### Boss 1: Rootweaver — Expedition Boss

| Property | Detail |
|----------|--------|
| Gems | Synthesis, Resonance, Flux |
| Phases | 2 |
| Rounds | 12-18 |
| Unique Resonance | "Living Architecture" — Synthesis + Resonance: constructs gain echo. Each construct tick echoes at 40% power 1 round later |

**Phase 1 (100-50%): The Growing**
- Boss creates constructs rapidly — healing, damage, and buff constructs overlap
- Flux gem means construct types are unpredictable (Metamorphosis shifts construct behavior)
- **Lesson:** Prioritize construct destruction. Entropy (Corrode, Unravel) is highly effective. Don't let the board fill up

**Phase 2 (50-0%): The Bloom**
- Living Architecture resonance activates. Every construct now echoes
- Boss uses Merge to combine constructs into mega-constructs with combined effects
- Catalyze becomes the burst threat — if boss has 3+ constructs and Catalyzes, the simultaneous effect + echo is devastating
- **Lesson:** Keep construct count low. If it reaches 3+, expect Catalyze. Commit defensive in the round after a Merge (the mega-construct hits hard)

#### Boss 2: The Thornmother — Expedition Boss

| Property | Detail |
|----------|--------|
| Gems | Synthesis, Resonance, Entropy |
| Phases | 3 |
| Rounds | 18-22 |
| Unique Resonances | "Rotting Garden" (Synthesis + Entropy), "Eternal Echo" (Resonance + Entropy) |

**Phase 1 (100-65%): Root Network**
- Thornmother creates a network of constructs that connect to each other
- Damaging one construct distributes some damage to connected constructs (no single-target focus)
- Entropy debuffs from the boss extend to players standing in construct zones
- **Lesson:** Position away from construct zones when attacking. The network makes single-target attrition slow — area effects or gem-breaking is more effective

**Phase 2 (65-35%): Decay Bloom**
- Rotting Garden activates: constructs now apply Entropy debuffs passively (defense reduction aura)
- Boss debuff stacking becomes aggressive — the longer the fight goes, the weaker players become
- Construct destruction now spawns a 1-round decay zone (damage area)
- **Lesson:** This phase has a soft timer via stacking debuffs. Break the Entropy gem to end the debuff escalation, or destroy constructs quickly (accepting the decay zone cost)

**Phase 3 (35-0%): Overgrowth**
- All mechanics active. Construct spawn rate doubles
- Eternal Echo: boss Resonance echoes apply Entropy debuffs (echo + decay combined)
- The arena fills with constructs. Players must create clear paths through construct zones to reach the boss
- Enrage: after 8 rounds in Phase 3, constructs become indestructible
- **Lesson:** Race phase. This is where burst damage and gem attrition payoff from earlier phases matters. A party that broke the Entropy gem in Phase 2 faces a dramatically easier Phase 3

---

### Void Nexus (Obsidian Wastes)

**Domain ecology:** Entropy / Void

#### Boss 1: Hollow Sentinel — Expedition Boss

| Property | Detail |
|----------|--------|
| Gems | Void, Entropy, Aegis |
| Phases | 2 |
| Rounds | 14-18 |
| Unique Resonance | "Consuming Shield" — Void + Aegis: Counter-Energy stored by the boss also drains stability from the attacking player's most-used gem |

**Phase 1 (100-50%): The Drain**
- Boss absorbs player buffs via Consume and converts them to Counter-Energy
- Entropy debuffs weaken player gems, accelerating mutation
- **Lesson:** Don't buff predictably — the boss will eat your buffs. Apply buffs AFTER the boss commits to a non-Consume action (read the intent)

**Phase 2 (50-0%): The Hollow**
- Null Zones appear, creating dead areas where player actions fail
- Consuming Shield resonance activates — attacking the boss's shield now costs YOUR gem stability
- Boss uses Siphon to steal player resources, fueling its own offense
- **Lesson:** Avoid hitting into Fortify (triggers Consuming Shield). Wait for the boss's offensive rounds to attack. Break the Aegis gem to disable the resonance entirely

#### Boss 2: The Void Arbiter — Expedition Boss

| Property | Detail |
|----------|--------|
| Gems | Void, Entropy, Kinetics |
| Phases | 3 |
| Rounds | 18-22 |
| Unique Resonances | "Event Horizon" (Void + Kinetics), "Devouring Decay" (Void + Entropy) |

**Phase 1 (100-65%): Judgement**
- Boss alternates between displacement (Kinetics) and negation (Void)
- Players pushed into Null Zones lose their committed actions (waste a round)
- Entropy debuffs stack, reducing player defense over time
- **Lesson:** Counter-position against displacement. Never stand adjacent to a Null Zone. The boss pushes you INTO them

**Phase 2 (65-35%): Sentencing**
- Event Horizon: Kinetics displacement now pulls ALL players toward a central point. If 2+ players occupy the same position, both take Void damage
- Devouring Decay: Entropy debuffs on players now also drain gem stability (-3 per debuff per round)
- **Lesson:** Spread out. Cleanse debuffs with Resonance (Dampening Wave) or Synthesis (construct buffs) if available. This phase punishes parties that neglect utility

**Phase 3 (35-0%): Annihilation**
- Boss uses Absence (untargetable for 1 round) followed by a full 3-action burst commit
- All Null Zones merge into one massive dead zone — half the arena is unusable
- Enrage: every 3 rounds, one player's most unstable gem loses 10 stability
- **Lesson:** Track when Absence is used in the log — the burst commit always follows. Defend on the round after Absence. The arena shrinkage forces aggressive play in limited space

---

### Radiant Sanctum (Celestial Peaks)

**Domain ecology:** Resonance / Flux

#### Boss 1: Harmonic Guardian — Expedition Boss

| Property | Detail |
|----------|--------|
| Gems | Resonance, Aegis, Synthesis |
| Phases | 2 |
| Rounds | 14-18 |
| Unique Resonance | "Perfect Harmony" — Resonance + Synthesis: constructs generate echoes that create NEW constructs (self-replicating) |

**Phase 1 (100-50%): Resonant Defense**
- Boss creates constructs that echo. Echoes create buff zones for the boss
- Defensive and utility focused — boss rarely attacks directly
- **Lesson:** This boss is deceptively dangerous. The passive scaling from construct echoes means doing nothing is losing. Aggressive pressure early prevents the board from spiraling

**Phase 2 (50-0%): Cascade**
- Perfect Harmony activates. Constructs self-replicate through echoes
- Boss switches to offensive — uses Catalyze + Reflect for burst damage powered by the construct network
- Arena can fill with constructs in 3-4 rounds if unchecked
- **Lesson:** This is a hard DPS + construct management check. Break the Synthesis gem to stop new constructs, or break Resonance to stop the echo replication. Either changes the fight dramatically

#### Boss 2: The Flux Arbiter — Expedition Boss

| Property | Detail |
|----------|--------|
| Gems | Flux, Resonance, Tempest |
| Phases | 2 |
| Rounds | 15-20 |
| Unique Resonance | "Chaos Resonance" — Flux + Resonance: boss echoes are transformed by Flux, making echo effects unpredictable (damage echo might become heal, debuff echo might become buff) |

**Phase 1 (100-50%): Adaptation**
- Boss mirrors player strategy via Flux (Shift Strike adapts to player actions)
- Resonance echoes build up. Players must track which echoes are pending
- **Lesson:** Vary your actions to prevent the boss from adapting effectively. Consistent patterns give Flux maximum power

**Phase 2 (50-0%): Chaos**
- Chaos Resonance activates. All pending echoes become unpredictable
- Tempest chains build alongside the chaos — chain burst + unpredictable echoes = high variance rounds
- Boss uses Metamorphosis on its own gems, shifting its action pool mid-fight
- **Lesson:** This is the "chaos management" boss. Certainty is gone. Players must commit to flexible strategies (conditional actions are king here). Break the Flux gem to restore predictability to echoes

#### Boss 3: The Radiant Arbiter — Expedition Boss (Signature)

| Property | Detail |
|----------|--------|
| Gems | Resonance, Flux, Entropy, Aegis |
| Phases | 3 |
| Rounds | 22-28 |
| Unique Resonances | "Resonant Decay" (Resonance + Entropy), "Adaptive Shield" (Flux + Aegis), "Convergence" (all 4 gems) |

**Phase 1 (100-65%): The Test**
- Boss evaluates player party composition. AI adapts priority table based on party's dominant domains
- Resonant Decay: echoes carry Entropy debuffs, degrading player gems over time
- Adaptive Shield: boss defense changes type each round based on what players attacked with last round
- **Lesson:** The boss adapts to you. Diverse party loadouts make this phase easier — mono-domain parties face amplified Adaptive Shield resistance

**Phase 2 (65-35%): The Judgement**
- Boss becomes aggressive. All 4 gems active in every commit (3 actions drawn from full pool)
- Entropy pressure intensifies — player gem stability is the primary threat, not Integrity
- Boss tries to break player gems before killing them
- **Lesson:** Protect your key gems. Stabilize aggressively (Aegis actions, Synthesis constructs). A party that loses key gems in this phase faces a nearly impossible Phase 3

**Phase 3 (35-0%): Convergence**
- Convergence resonance: if all 4 boss gems are above 50 stability, the boss gains a 4th action slot for 3 rounds (quadruple commit)
- This is the gem attrition payoff check — a party that destabilized boss gems earlier prevents Convergence
- If Convergence triggers, the 3-round window is devastating but survivable with defensive play
- Arena-wide echo cascade: all pending echoes fire simultaneously, then repeat each round
- **Lesson:** This is the ultimate test of everything — attrition management, defensive timing, burst damage, and log reading. Parties that prevented Convergence face a difficult but fair fight. Parties that didn't face a near-wipe that tests pure survival

---

### MVP World Boss: The Thornmother (Verdant Depths)

| Property | Detail |
|----------|--------|
| Gems | Synthesis, Resonance, Entropy, Kinetics |
| Scaling | 10-50 players |
| Phases | 4 |
| Rounds | 30-50 |
| Duration | 10-18 min |
| Spawn | 2-hour timer, World Boss Herald announces 30 min prior |

**Phase 1 (100-75%): Awakening**
- Thornmother creates root constructs across the arena — these are the primary mechanic for the entire fight
- Accessible to new players: clear telegraphs, generous timing, forgiving damage
- Community coordination begins: players naturally spread across root construct positions

**Phase 2 (75-50%): Growth**
- Root constructs connect into a network. Destroying connected roots deals cascade damage to the boss
- Entropy debuffs begin targeting highest-contribution players (soft aggro mechanic)
- Kinetics displacement launches thorns at distant players (encourages staying within medium range)

**Phase 3 (50-25%): Rot**
- Network reverses — constructs now drain player gem stability instead of being destructible
- Players must displace constructs (Kinetics) or negate them (Void) instead of destroying them
- Boss resonance "Living Decay" activates: construct network pulses Entropy damage every 2 rounds

**Phase 4 (25-0%): Bloom**
- All mechanics active. Construct spawn rate triples
- Boss gains Catalyze (triggers all constructs for burst). Telegraphed 2 rounds in advance
- Enrage at 10 minutes: constructs become indestructible, damage ramps +10% per round
- **Spectacle moment:** when Thornmother dies, all constructs bloom into healing zones, restoring all players to full

**Contribution rewards:** See [[Loot System]] for world boss contribution thresholds and drop tables.

---

## Boss Difficulty Scaling

### Challenge Mode Modifiers

Expedition bosses gain additional mechanics under [[PvE Structure#Challenge Mode]]:

| Modifier | Effect on Bosses |
|----------|-----------------|
| **Volatile Gems** | Boss gems mutate faster. Priority tables gain mutation-conditional entries |
| **Information Blackout** | Boss intent display shows 1 fewer hint. Domain detection delayed by 1 additional use |
| **Momentum Lock** | Player momentum capped at 7 (no Dominance). Boss momentum floor raised to 3 |
| **Resonance Amplification** | Boss resonance effects deal +25% damage/potency. Player resonances also amplified |
| **Stability Siege** | All boss actions drain +3 additional stability from player gems |
| **Speed Inversion** | Slower actions resolve first (reverses speed priority calculation) |

### Behavioral Adaptation

From [[Behavioral Identity#Dungeon AI Adaptation]], boss AI subtly adjusts to party behavioral profile:

| Party Lean | Boss Adjustment |
|------------|----------------|
| High Aggression | Boss defensive priorities escalate. Boss adds spawn slightly earlier |
| High Defense | Boss aggression increases +10%. Enrage timers tighten by 5% |
| High Prediction | Boss telegraphs rotate between 2-3 variants instead of fixed sequence |
| High Resonance Seeking | Dungeon gem ecology becomes more volatile |
| High Mutation Affinity | Gem stability drain +10%, but mutations bias toward beneficial |

---

## Design Pipeline

Boss encounters are the development bottleneck (see [[PvE Structure#What This Means for Development]]). Each boss requires:

| Component | Effort |
|-----------|--------|
| Priority table design (per phase) | Core design |
| Unique action set (3-6 boss-exclusive actions) | Combat design + balance |
| Unique resonance design (1-2 per boss) | Systems design |
| Phase transition logic and narration | Narrative + systems |
| Intent display text and hints | UX writing |
| Combat log narration (flavor text per action) | Narrative |
| Challenge Mode modifier interactions | Balance |
| Behavioral adaptation tuning | QA + balance |
| Playtest and iteration | QA (largest time cost) |

**Target:** 2-3 weeks per Expedition boss, 1 week per mini-boss, 4-5 weeks per Mythic boss, 3-4 weeks per world boss.

---

## Related Pages

- [[Combat System]] — Phase-based combat, boss gem ecology, PvE combat rules
- [[Combat Logs System]] — Boss mechanic log lines, intent display, resonance signals
- [[PvE Structure]] — Dungeon categories, boss design philosophy, Challenge Mode
- [[Gem Identity System]] — Domain actions and systemic rules that bosses use
- [[Behavioral Identity]] — AI adaptation based on party behavioral profile
- [[World Design]] — Dungeon locations and domain ecology per zone
- [[Loot System]] — Boss drop tables and contribution rewards
- [[NPC System]] — World Boss Herald
- [[UI Design]] — Enemy intent panel layout
