---
tags:
  - gdd
  - content
  - onboarding
---

# Onboarding System

## Design Philosophy

> *"The longest tutorial ever designed to not feel like one."*

The journey from level 1 to level 50 is a 20-hour teaching arc. Every system in Stormbound Online --- gem domains, mutation, stability, resonance, momentum, conditional actions, alignment, crafting --- is introduced through play, not through text walls, forced popups, or mandatory reading.

### Core Principles

1. **Teach through consequence, not instruction.** The player learns that stability matters because their gem mutated mid-fight and something unexpected happened --- not because a tooltip told them it would.
2. **One system per lesson.** Never introduce two mechanics in the same encounter. Each new concept gets its own moment.
3. **Captain Maren Voss is the thread.** She does not lecture. She gives you a job to do, and the job teaches you. Her dialogue is short, dry, and practical.
4. **Failure is a teacher.** Encounters designed to teach mechanics should allow (even encourage) failure states that demonstrate why the mechanic matters.
5. **The player should never feel lost, but should always feel like they discovered something themselves.** The system provides guardrails, not rails.
6. **Respect the veteran.** Returning players and alt characters can skip or accelerate onboarding content. Nobody is forced through a tutorial they have already mastered.

### What We Do NOT Do

- No mandatory tutorial popups longer than one sentence
- No "click here to continue" overlays that block gameplay
- No NPC monologues
- No locking the player in a room until they "complete the lesson"
- No explaining systems before the player has encountered them organically

---

## First Hour Experience (Beat-by-Beat)

The first 60 minutes take the player from character creation to level 5-7, unlocking their second gem slot and establishing the core combat loop. Pacing target: levels 1-5 in the first 30 minutes.

### Minutes 0-5: Arrival

**What the player sees:**

The screen opens on Port Haven. Descriptive text establishes the harbor atmosphere --- warm breeze, gulls, the hum of Tova Ironhand's workshop. The UI is minimal: a text panel, a single action button labeled "Look Around," and an ambient sound cue.

**What the player does:**

- Selects "Look Around" --- the UI expands to show Port Haven as a hub with context-sensitive action buttons
- A notification appears: *"Someone is waving you over from the docks."*
- Player selects the notification --- transitions to Captain Maren Voss's location

**What Maren says:**

> "You look like you just washed in. Good. I need someone who doesn't know enough to say no. Come with me."

**Design note:** No origin selection yet. The player meets a character first. Investment before commitment.

### Minutes 5-10: Origin Selection

**What the player sees:**

Maren leads the player to the Training Grounds. Sparring Master Holt is running drills. Maren presents the origin choice as a practical question, not a character creation screen.

**What Maren says:**

> "Holt's going to size you up. Tell him where you come from --- it changes how you fight."

**What the player does:**

- The UI presents the 6 Origins ([[Experience & Levelling#6 Origins]]) as selectable cards
- Each card shows: name, one-line lore description, starting stat bias (displayed as a simple bar graph), passive ability name and one-sentence description
- No "best choice" is implied. All cards are visually equal
- Player selects an Origin. Confirmation prompt: *"This is permanent. Your origin shapes your instincts, not your limits."*

**What happens after selection:**

- Brief text narration acknowledging the choice (unique per origin, 2 sentences)
- Stats apply silently. No stat screen appears yet --- the player does not need numbers at this point

### Minutes 10-15: First Gem

**What the player sees:**

Maren takes the player to Tova Ironhand's workshop. Tova is working on a gem.

**What Maren says:**

> "Pick one. Don't overthink it --- you'll find more."

**What the player does:**

- Tova presents 3 gem domain choices (curated based on Origin to avoid overwhelming with 8 options)
  - Each Origin sees a different curated set of 3, weighted toward domains that complement the Origin passive
  - Example: Stormborn sees Tempest, Kinetics, and Flux. Ironsworn sees Aegis, Kinetics, and Synthesis
- Each gem is presented as a card with: domain name, one-line flavor quote (from [[Gem Identity System]]), and the names of its 4 base actions (no detailed descriptions yet)
- Player selects a gem. It equips automatically into Gem Slot 1

**What Maren says after:**

> "Good. Now let's see if you can use it."

### Minutes 15-25: First Combat

**What the player sees:**

Maren directs the player to the Training Grounds where Sparring Master Holt has a practice opponent ready --- a Training Construct (Aegis-domain, low difficulty, telegraphed intent).

**What happens:**

The first fight is a 3-round PvE encounter with heavy scaffolding:

**Round 1: Draw Phase Tutorial**

- The Draw phase triggers. The player sees their 4 drawn actions for the first time
- A single contextual hint appears at the top of the combat panel: *"These are your options this round. Pick 2."*
- Enemy intent is fully visible: `[Defensive] -> [Defensive]`
- The player selects 2 actions and the Commit timer is generous (15 seconds instead of the standard 8-10)
- Clash resolves. The combat log narrates what happened in clear, readable prose
- Aftermath shows simplified results: damage dealt, damage taken, stability unchanged

**Round 2: Ordering Matters**

- Draw phase triggers again. Contextual hint: *"The order you pick matters. First action resolves first."*
- Enemy intent shifts: `[Offensive] -> [Defensive]`
- The player experiments with action ordering
- Combat log narrates the sequence, showing how order affected the outcome

**Round 3: Reading the Log**

- No hint this round. The player is expected to read the combat log from rounds 1-2 and make an informed choice
- Enemy intent: `[Offensive] -> [Offensive]`
- If the player wins: Maren comments on their performance. If the player loses: Maren is unimpressed but not punishing --- *"You'll live. Barely. Let's go again."* (Fight resets)

**Reward:** Small XP grant (enough to reach level 2), first gold, and a brief combat summary panel showing rounds played and actions used.

### Minutes 25-35: Port Haven Exploration

**What the player sees:**

After the first fight, Maren sends the player to explore Port Haven independently. The hub reveals its full action set: NPC interactions, the marketplace (browse only at this level), and the notice board.

**What Maren says:**

> "Go talk to people. Find out where the work is. I'll be at the docks."

**What the player does:**

- Explores Port Haven through context-sensitive buttons
- Meets 2-3 ambient NPCs who react to the player's Origin
- Discovers the notice board with available bounties (first bounty is pre-selected: a Shallow Caves run)
- Can inspect Tova's shop (items visible but most require higher level)
- Can visit Sparring Master Holt for optional practice fights

**Design note:** This is a decompression beat. No combat, no tutorials. The player absorbs the world at their own pace. Social systems are visible (other players in the hub) but not pushed.

### Minutes 35-50: Shallow Caves (First Dungeon)

**What the player sees:**

The player enters Shallow Caves --- the introductory farming dungeon. 3 encounters, no boss. Aegis-domain ecology (forgiving environment per [[World Design#Calm Shores]]).

**What happens:**

- **Encounter 1:** Standard 3-round fight against a Tidepool Crab. No new mechanics. Reinforces Draw/Commit/Clash/Aftermath flow
- **Encounter 2:** Two enemies. The player must choose which to target (target selection introduced). One enemy is weak to their domain, one is neutral. The combat log hints at the advantage: *"The crab's shell cracks more easily under [domain] pressure."*
- **Encounter 3:** A slightly tougher enemy that uses an action the player has not seen before. After the fight, the combat log describes the unfamiliar action clearly. This is the player's first encounter with information warfare --- learning from what the enemy did

**Loot:** First gem drop (random domain, low rarity). The gem goes to inventory. The game does NOT prompt the player to equip it yet --- they only have 1 slot. This creates anticipation for the 2nd slot unlock.

**XP:** Enough to reach level 3-4.

### Minutes 50-60: The Hook

**What the player sees:**

Returning to Port Haven, the player finds Maren at the docks. She has a new job.

**What Maren says:**

> "There's something in Tidecaller Caverns that shouldn't be there. The Trade Guild wants it cleared. You're not ready, but I'm not asking."

**What happens:**

- The player receives their first real quest: "The First Spark" Quest 1 (see questline below)
- Before they can enter Tidecaller Caverns, Maren directs them to Tova for a gem inspection. Tova notices the gem the player found in Shallow Caves
- Tova explains (in 1 sentence) that the player will need a second gem slot soon. The level 5 unlock is foreshadowed, not explained

**End of first hour state:**

- Level 3-5
- 1 gem equipped, 1 gem in inventory
- Combat fundamentals understood: Draw, Commit, Clash, Aftermath
- Port Haven explored
- First quest active
- Second gem slot anticipated but not yet unlocked
- No systems explained that have not been experienced

---

## System Introduction Sequence

Each major system is introduced at a specific level, tied to a gameplay moment that demonstrates the mechanic before explaining it. Systems map to the unlock schedule defined in [[Experience & Levelling#Level Table]].

### Level 1-5: Foundation

| Level | System | How It Is Taught |
|---|---|---|
| 1 | Origin selection | Character creation (Maren + Holt framing) |
| 1 | First gem domain | Tova presents curated choice of 3 |
| 1-2 | Draw/Commit/Clash/Aftermath | First combat encounter with scaffolded hints |
| 2-3 | Action ordering | Second combat round, contextual hint |
| 3-4 | Combat log reading | Third+ combat encounters, hints removed |
| 5 | **2nd gem slot** | Tova equips the second gem. First experience of expanded draw pool |

**Maren's role (Levels 1-5):** Direct guide. She tells you where to go and what to do. Her dialogue is imperative: *"Go here. Do this. Come back."*

**Practice encounter at Level 5:** After equipping the 2nd gem, Sparring Master Holt offers a practice fight specifically designed to show the expanded draw pool (now 6-8 actions drawn from). The player sees more options and must adapt.

### Level 5-10: Expansion

| Level | System | How It Is Taught |
|---|---|---|
| 5 | Expanded draw pool | Practice fight with 2 gems |
| 5 | Basic crafting | Tova gives first crafting quest (shape a gem, 1-step recipe) |
| 6-7 | Stability (introduction) | A fight where stability drops to Shifting for the first time. The combat log describes the mutation: *"Your [gem] flickers. Something shifts inside it."* |
| 7-8 | Mutation (first experience) | A guided encounter where the player's gem reaches Shifting threshold. Post-combat, Tova explains Revert vs Lock In (2 sentences) |
| 8-9 | Momentum (basic) | A fight against an enemy that builds momentum. The player sees their own momentum bar shift for the first time. Draw count changes (4 vs 3) demonstrated through gameplay |
| 10 | **3rd gem slot** | Earned through quest reward. Three domains equipped simultaneously |
| 10 | **PvP unlocked** | Admiral Kael Stormwright's envoy arrives in Port Haven. Arena invitation delivered |

**Maren's role (Levels 5-10):** Hands-off mentor. She checks in periodically but sends the player on independent missions. Her dialogue shifts to observation: *"You're starting to read the log. Good. Most don't bother."*

**Practice encounter at Level 10:** A practice duel against a training partner NPC (not Holt --- a named sparring NPC) that demonstrates how 3 gems expand strategic options. The fight is designed so the player wins if they use actions from all 3 domains.

### Level 10-15: Depth

| Level | System | How It Is Taught |
|---|---|---|
| 10-11 | PvP basics | First PvP match is an unranked tutorial duel with a matched-level opponent. No rating on the line |
| 11-12 | Resonance (discovery) | During a dungeon fight, a resonance triggers for the first time. The combat log uses distinctive language: *"The echo answers with something new."* Post-combat, a Codex entry unlocks. Maren: *"That's a resonance. Your gems talked to each other. It won't happen every time."* |
| 12-13 | Information warfare | A PvE encounter where enemy domain is hidden until first use. The combat log becomes the primary information source |
| 13-14 | Gem ecology (zone effects) | Player transitions to content where domain ecology affects combat. A brief environmental description mentions the dominant domain. Their first fight in a non-Aegis zone demonstrates the difference |
| 15 | **4th gem slot (full loadout)** | Unlocked through "The First Spark" questline climax. All 4 gem slots active |
| 15 | **Stormreach access** | Travel to Stormreach becomes available |

**Maren's role (Levels 10-15):** Colleague. She speaks as an equal. Her dialogue includes tactical observations: *"Your Entropy gem's been shifting. If you let it go deeper, it might surprise you --- or break. Your call."*

### Level 15-25: Mastery

| Level | System | How It Is Taught |
|---|---|---|
| 15-17 | Full loadout optimization | Stormreach content assumes 4 gems. Encounters punish single-domain spam |
| 17-19 | Momentum thresholds | Stormreach Arena fights are tuned to reach Advantage and Pressure zones regularly. The player experiences Draw 5 and Draw 3 organically |
| 20 | **Modifier gem slots expand** | Modifier gems introduced through Lumen at Lightning Forge |
| 20 | **First faction introduction** | Region faction reputation begins accumulating passively |
| 22-24 | Overcharge (preview) | An NPC opponent uses Overcharge against the player. The combat log describes the trade-off. The player sees the mechanic before they can use it |
| 25 | **Alignment system unlocked** | Alignment choice presented through a narrative quest. Lightkeeper Asha, Voidcaller Myx, and Wanderer Syl each make their case. Player chooses. No pressure --- depth 0-20 switching is free |

**Maren's role (Levels 15-25):** Maren's questline is complete at level 15. She remains available at Port Haven for optional dialogue that reacts to your progress. She does not appear in Stormreach content --- Admiral Kael Stormwright and Lumen take over as region guides.

### Level 25-50: Independence

| Level | System | How It Is Taught |
|---|---|---|
| 25-29 | Alignment depth progression | Alignment quests from faction NPCs. Gem modifications begin appearing |
| 30 | **Mythic dungeons** | Dungeon Guide Pip introduces Mythic tier. First Mythic run is a curated experience with guided difficulty |
| 30 | **Overcharge unlocked** | Player can now Overcharge. First opportunity presented in a Mythic fight where it is clearly advantageous |
| 35 | **Build publishing** | Creator ecosystem opens. Build codes shareable |
| 35 | **Conditional actions unlocked** | A solo trial encounter is designed to be nearly impossible without using conditional logic. After the player fails or barely wins, the conditional system unlocks and the trial becomes completable. The system teaches itself through the gap between failure and success |
| 40 | **Faction reputation fully active** | Faction vendors, reputation rewards, daily bounties expand |
| 45-50 | Endgame preparation | Content difficulty assumes mastery of all systems |

---

## "The First Spark" Questline

Captain Maren Voss's tutorial questline spans levels 1-15, comprising 18 quests. Each quest teaches one core mechanic. The questline name refers to the first resonance the player discovers --- the "spark" between two gems that creates something neither could alone.

### Quest Design Rules

- Every quest has ONE mechanic focus
- Quest objectives are completable in 5-15 minutes
- Maren's dialogue per quest is 3-5 lines maximum
- Rewards always include XP plus one thematic item or unlock
- No quest requires the player to read external documentation

### Quest Chain

| # | Quest Name | Level | Objective | Mechanic Taught | Reward | Maren's Theme |
|---|---|---|---|---|---|---|
| 1 | "Prove You Float" | 1 | Defeat the Training Construct at the Training Grounds | Draw/Commit/Clash/Aftermath phases | First gold, XP to level 2 | Pragmatic assessment: *"You didn't die. That's a start."* |
| 2 | "Tova's Scraps" | 2 | Visit Tova Ironhand, select your first gem | Gem selection, gem equipping | First gem (player choice of 3) | Practical introduction: *"Tova's the best gem shaper on the island. Don't tell her I said that."* |
| 3 | "Something in the Shallows" | 3 | Clear Shallow Caves (3 encounters) | Multi-encounter dungeon flow, target selection | Gem drop (random domain), gold | Sending you off: *"The caves won't kill you. Probably."* |
| 4 | "Read the Wreckage" | 3 | Replay any encounter and correctly identify one enemy action from the combat log (verified by post-combat quiz) | Combat log reading | Codex: Combat Log entry, XP | Teaching observation: *"The log tells you everything. Most fighters never read it. Don't be most fighters."* |
| 5 | "What's Left Behind" | 4 | Bring the gem found in Shallow Caves to Tova for inspection | Gem inventory, gem inspection, gem properties (rarity, domain) | Gem appraised (value shown), small gold | Anticipation: *"Hold onto that. You'll need a second slot before it's useful."* |
| 6 | "Two Voices" | 5 | Equip your 2nd gem and win a practice fight using actions from both domains | 2nd gem slot, expanded draw pool, multi-domain usage | 2nd gem slot permanent unlock, XP | Milestone: *"Two gems. Two voices. Now you're starting to sound like a fighter."* |
| 7 | "Tova's Lesson" | 5 | Complete your first crafting recipe at Tova's workshop | Basic crafting (Gem Shaping, 1-step recipe) | Crafted gem (minor quality), crafting XP | Crafting hook: *"Building something yourself beats finding it in a cave. Usually."* |
| 8 | "The Fraying Edge" | 6 | Complete a fight where at least one gem reaches Shifting stability | Stability system, mutation thresholds | Stabilizer consumable (x3), XP | Stability awareness: *"Feel that? Your gem's starting to change. That's not always bad."* |
| 9 | "Keep or Kill" | 7 | After a fight with a mutated gem, choose Revert or Lock In | Post-combat mutation choices (Revert vs Lock In) | XP, gold. Outcome varies by choice | Agency: *"Nobody can tell you which version is better. That's the point."* |
| 10 | "Riding the Wave" | 8 | Win a fight after reaching Momentum 7+ at least once | Momentum system, momentum thresholds, Draw 5 | Momentum Codex entry, XP | Tactical awareness: *"Momentum isn't luck. It's reading the fight and acting on what you see."* |
| 11 | "Tidecaller's Gate" | 9 | Enter Tidecaller Caverns and clear the first floor (2 encounters + mini-boss) | Expedition dungeon structure, boss intent reading, higher difficulty PvE | Rare gem drop, gold, XP | Escalation: *"The caverns are real. No practice dummies. Watch the intent display --- it won't tell you everything, but it tells you enough."* |
| 12 | "The Third Voice" | 10 | Equip your 3rd gem and defeat a Tidecaller Caverns encounter using actions from all 3 domains | 3rd gem slot, three-domain synergy | 3rd gem slot permanent unlock, XP, gold | Growth: *"Three domains. The draw pool gets crowded. Learn to choose fast."* |
| 13 | "Storm on the Horizon" | 10 | Speak to Admiral Kael's envoy at Port Haven. Accept the arena invitation | PvP system introduction, arena concept | Arena access token, XP | Transition: *"Kael's people want to see what you've got. PvP is different --- the enemy reads the log too."* |
| 14 | "First Blood" | 10-11 | Complete one unranked PvP duel | PvP combat (hidden enemy intent, information warfare, real opponent) | PvP honor points (first grant), XP | PvP reality: *"Win or lose, you learned something. That's the only currency that matters in the arena."* |
| 15 | "The Echo Answers" | 11-12 | Trigger a resonance effect during any combat encounter | Resonance system (discovery-based, emergent from gem pairing) | Resonance Codex entry, resonance fragment, XP | Discovery: *"That's a resonance. Your gems found something between them. Chase that feeling --- it's where the real builds live."* |
| 16 | "Stranger Tides" | 12-13 | Complete an encounter in a non-Aegis zone (gem ecology area near Port Haven outskirts) | Zone gem ecology, domain resonance bonus/suppression | Domain ecology Codex entry, zone-specific gem, XP | Environmental awareness: *"Different places treat your gems differently. The world isn't neutral."* |
| 17 | "The Full Set" | 14-15 | Acquire and equip a 4th gem. Defeat the Tidecaller Caverns final boss with a full 4-gem loadout | 4th gem slot, full loadout strategy, boss encounter | 4th gem slot permanent unlock, rare gem, significant XP + gold | Culmination: *"Four gems. Full loadout. You're not a student anymore."* |
| 18 | "The First Spark" | 15 | Trigger a resonance you have not triggered before, using your full 4-gem loadout. Return to Maren | Full system mastery checkpoint, resonance as the creative core | Title: "Sparkbearer," unique cosmetic nameplate accent, Stormreach travel unlocked | Farewell: *"That spark between your gems --- that's the real game. Everything before this was just making sure you'd survive it. Go find Kael in Stormreach. Tell him I sent you. And read the damn log."* |

### Post-Questline: Maren's Persistence

After completing "The First Spark," Captain Maren Voss remains at the Port Haven docks. She offers:

- **Reactive commentary** on the player's current build, alignment choice, and progression milestones
- **Optional repeatable bounties** for Calm Shores content
- **Dialogue that evolves** as the player progresses (references Stormreach events, alignment choices, prestige levels)
- **A hidden interaction** at Prestige 60: Maren acknowledges the player has surpassed her. One line. No fanfare: *"I used to be the best fighter on this dock. Used to be."*

---

## Contextual Help System

The game detects when a player is struggling and responds with graduated assistance. Help is offered, never forced. The system respects player agency and intelligence.

### Detection Triggers

| Signal | Condition | Confidence |
|---|---|---|
| Repeated losses | 3+ consecutive losses in same content tier | High |
| Unused mechanics | Player has unlocked a system (e.g., conditional actions) but has not used it in 10+ fights | Medium |
| Long Commit pauses | Player consistently uses >80% of the Commit timer for 5+ rounds | Medium |
| Single-domain spam | Player uses actions from only 1 gem domain for 5+ consecutive rounds despite having 2+ gems | High |
| Stability ignorance | Player's gem reaches Critical (25-1) without ever using a stabilizer or reverting a mutation | Medium |
| Momentum blindness | Player never adjusts strategy when entering Pressure or Desperation zones | Medium |
| Log skipping | Player commits within 1 second of Draw phase completing, consistently (suggests not reading the log) | Low |

### Response Hierarchy

The system escalates through 4 tiers. It never jumps to Tier 3 or 4 without exhausting lower tiers first.

**Tier 1: Environmental Hint**

- The combat log includes slightly more descriptive language about relevant mechanics
- Example (stability): *"Your Tempest gem trembles at the edges. The storm inside it is less predictable now."*
- Example (momentum): *"The momentum of the fight shifts away from you. Your options narrow."*
- Feels like narrative, not instruction

**Tier 2: NPC Aside**

- After the fight, a relevant NPC offers a single line of tactical advice
- Sparring Master Holt (combat): *"Try mixing domains. One-note fighters are easy to read."*
- Tova (stability): *"Your gem's been through a lot. Bring it by the shop before it breaks."*
- Maren (general): *"Read the log. The answer's in there."*
- Appears as a brief text notification, dismissable

**Tier 3: Codex Highlight**

- The relevant Codex entry gains a subtle "NEW" indicator in the UI
- If the player opens the Codex, the entry for the mechanic they are struggling with is surfaced at the top
- No popup. No forced interaction. Just visibility

**Tier 4: Practice Prompt**

- After 5+ losses at the same content, Sparring Master Holt appears in the hub with a targeted practice encounter
- The practice encounter is specifically designed to teach the mechanic the player is missing
- Accepting is optional. The prompt disappears after the player's next win at any content

### Anti-Patronizing Rules

- No system ever says "It looks like you're having trouble"
- No system reduces difficulty without the player requesting it
- Hints never appear during PvP (competitive integrity)
- The same hint never appears twice in a session
- If a player dismisses hints 3 times, hint frequency reduces by 50% permanently (stored in preferences, reversible in settings)
- Players can disable the contextual help system entirely in Settings

---

## Codex / Reference System

The Codex is an in-game encyclopedia that fills in as players discover mechanics. It is NOT a pre-loaded wiki. Every entry is earned through play.

### Codex Structure

| Section | Content | Unlock Condition |
|---|---|---|
| **Domains** | One page per gem domain (systemic rule, action list, mutation tendency) | Equip a gem of that domain for the first time |
| **Combat** | Phase descriptions, momentum thresholds, stability states | Complete the relevant tutorial encounter or reach the relevant game state |
| **Resonance** | One entry per discovered resonance (triggering domains, tier, effect description) | Trigger the resonance in combat |
| **World** | Region descriptions, dominant domains, key locations | Visit the region |
| **NPCs** | Character bios, faction affiliations, service descriptions | Interact with the NPC |
| **Crafting** | Recipe lists, discipline descriptions, material sources | Learn the recipe or discover the material |
| **Alignment** | Alignment descriptions, depth tiers, switching costs | Unlock the alignment system (Level 25) |
| **Bestiary** | Enemy descriptions, domain affiliations, known actions | Encounter the enemy type |

### Codex Design Rules

- Entries are written in the voice of a scholar within the world (in-universe framing)
- No entry exceeds 200 words. Brevity is sacred
- Entries cross-link to related entries (wikilink-style within the Codex)
- Undiscovered entries show as `[???]` in the Codex index --- the player knows something exists but not what it is. This creates curiosity
- Resonance entries intentionally omit exact trigger conditions. They describe the effect and the participating domains, but HOW to trigger it remains the player's domain. This preserves the discovery-driven design from [[Gem Identity System#Resonance Discovery]]
- The Codex tracks discovery percentage per section. Completionists have a clear target

### Codex Unlock Moments

Key Codex entries unlock with a brief notification:

```
CODEX UPDATED ──────────────────────
  New Entry: Resonance — Harmonic Storm
  "When Tempest chains meet Resonance echoes,
   the storm sings a second verse."
  ▸ View Entry    ▸ Dismiss
─────────────────────────────────────
```

The notification is brief, non-blocking, and dismissable. It appears after combat ends, never during.

---

## Tooltip System

Information in the UI is layered. Players access increasing depth on demand.

### Three Layers

**Layer 1: Glance (always visible)**

- Action name, domain icon, one-word category (Offensive / Defensive / Utility)
- Example: `Storm Strike [Tempest] — Offensive`
- Visible in the Draw phase action cards, build panel, and gem inspection

**Layer 2: Detail (hover/inspect)**

- Full mechanical description of the action
- Current values based on player stats and gem rarity
- Domain tag, stability cost per use
- Example:

```
Storm Strike [Tempest]
─────────────────────────────────
Damage: 145-187 (scales with Force)
Effect: +1 Chain stack on hit
Stability cost: -3 per use
Domain: Tempest
─────────────────────────────────
```

- Accessible by hovering over an action card (desktop) or long-pressing (mobile)

**Layer 3: Advanced (expanded view)**

- Interaction notes: how this action behaves with specific resonances, mutation states, or conditional setups
- Historical performance: average damage dealt with this action over recent fights
- Domain ecology notes: how the current zone affects this action
- Example:

```
Storm Strike [Tempest] — Advanced
─────────────────────────────────
Resonance interaction:
  With Kinetics: displacement distance
  increased by Chain stack count

Mutation note (Shifting):
  May become "Frayed Strike" — reduced
  Chain gain but adds DoT component

Zone: Stormreach (Tempest dominant)
  +5 starting stability on this gem
  Mutation pressure: +20% faster

Recent performance:
  Avg damage: 162 (last 10 fights)
  Hit rate: 94%
─────────────────────────────────
```

- Accessible by clicking "More Info" from Layer 2, or via the Codex entry

### Tooltip Progression

- **Levels 1-5:** Only Layer 1 and Layer 2 are available. Layer 3 is hidden to avoid overwhelming new players
- **Levels 5-15:** Layer 3 becomes available but is not highlighted. Players who seek it find it
- **Level 15+:** All layers visible. Advanced tooltips reference resonances, zone ecology, and mutation states

### Tooltip Rules

- Tooltips never block the Commit timer. They appear adjacent to the action card, not overlaid
- Tooltip text uses the same color language as the rest of the UI (domain colors, rarity colors, alignment tints)
- Tooltips update dynamically mid-combat to reflect current stability state and mutation effects
- In PvP, tooltips for opponent actions are never available (information warfare)

---

## Returning Player Catch-Up

Players who leave for extended periods need re-orientation without being treated as new players.

### Absence Detection

The system tracks time since last login. Responses scale with absence duration.

| Absence | Category | Response |
|---|---|---|
| < 7 days | Short | No intervention |
| 7-30 days | Medium | Light recap on login |
| 30-90 days | Long | Structured catch-up flow |
| 90+ days | Extended | Full re-onboarding option |

### Medium Absence (7-30 days)

**On login, the player sees:**

A single recap panel (dismissable) containing:

- Current character state: level, equipped gems, alignment, region
- "What Changed" summary: patch notes condensed to 3-5 bullet points covering meta-relevant changes
- Active events: any weekly/monthly/seasonal events currently running
- Suggested action: one recommended next step based on their last session (*"You were mid-run in Stormspire. Resume?"* or *"The Mythic rotation changed. New dungeon: [name]."*)

### Long Absence (30-90 days)

**On login, the player sees:**

Everything from Medium, plus:

- **Build Check:** A notification that their gem loadout may be affected by balance changes. Links to a summary of domain changes relevant to their equipped gems
- **Sparring Master Holt offers a warmup:** A no-stakes practice fight to re-learn the combat flow. Fully optional
- **Codex highlights:** Any new Codex entries that were added to the game during absence are flagged with a "NEW" indicator (not the same as undiscovered entries --- these are entries the player COULD have had but the game added post-departure)

### Extended Absence (90+ days)

**On login, the player sees:**

Everything from Long, plus:

- **"What's New" guided tour:** An optional, skippable sequence where Maren Voss (if in Calm Shores) or the regional story NPC walks through major system additions. Each system addition is presented as a single interaction, not a tutorial chain
- **Gear/gem audit:** The game flags if any equipped gems have been reworked, rebalanced, or if new domains have been added. Tova (or the regional crafting NPC) offers to inspect the player's loadout and explain changes
- **Free respec token:** If significant balance changes occurred, the player receives a one-time free gem reforging at Tova's workshop. Acknowledgment that the game changed under them
- **Option: "Start Fresh":** For players who feel too lost, an option to replay "The First Spark" questline at current level with scaled rewards. This is NOT a level reset --- it is a narrative re-run for re-learning. Available once per 90-day absence

### Returning Player Rules

- No forced tutorials on return. Every element is optional and dismissable
- No "welcome back" rewards that create FOMO for active players. Returning players catch up through play, not gifts
- The game never punishes absence. No decayed stats, no lost progress, no expired items
- Rested XP ([[Experience & Levelling#Rested XP]]) serves as the natural catch-up mechanic for level progression
- Alt characters who have already completed onboarding can skip "The First Spark" entirely with a single "Skip Tutorial" option at character creation (only available if the account has a level 15+ character)

---

## Related Pages

- [[Experience & Levelling]] --- Level curve, Origins, XP sources, unlock schedule
- [[Combat System]] --- Phase-based combat, momentum, stability, mutation
- [[Gem Identity System]] --- 8 gem domains, resonance, archetype naming
- [[NPC System]] --- Captain Maren Voss, service NPCs, dialogue design
- [[World Design]] --- Regions, gem ecology, zone progression
- [[Alignment System]] --- Three alignments, depth tiers, switching costs
- [[Gameplay Loops]] --- Session, weekly, and meta loop structure
- [[MVP Design]] --- MVP scope and priorities
- [[UI Design]] --- Combat panels, tooltip format, hub interface
- [[Crafting System]] --- Disciplines, recipes, crafting flow
- [[Creator Ecosystem]] --- Build publishing, community systems