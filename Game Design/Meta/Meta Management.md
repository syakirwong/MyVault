---
tags:
  - gdd
  - meta
  - balance
  - live-ops
---

# Meta Management

This document defines how the competitive and PvE meta is monitored, shaped, and rotated. The goal is a meta that evolves through three forces — environmental pressure, community pressure, and developer intervention — so no single build dominates indefinitely and players always have reasons to experiment.

For balance formulas, see [[Gem System Math]].
For the competitive framework, see [[PvP Structure]].
For the build-sharing layer, see [[Creator Ecosystem]].

---

## Design Principles

1. **Rotation over nerfs.** Prefer environmental changes that shift what's optimal over direct stat reductions. Players should feel the world changed, not that their build was taken from them.
2. **Transparency over mystery.** Publish data. Let the community see what's dominant. Counter-building is a feature, not a bug.
3. **Slow hand, firm hand.** Tolerate a dominant build for 2-4 weeks before intervening. Quick nerfs punish early adopters. Slow nerfs let counter-play develop naturally. But when intervention is needed, act decisively.
4. **Never gut, always redirect.** A nerfed build should remain playable. The target is reducing a 65% win rate to 55%, not reducing it to 40%.
5. **Reward adaptation.** Systems that force build changes (weekly rotations, seasonal modifiers) are preferable to reactive patches.

---

## Meta Control Levers

The game has five categories of meta control, ordered from lightest to heaviest intervention.

### Tier 1: Automatic Rotation (No Dev Action Required)

These systems run on schedule and continuously shift what's optimal.

| System | Cadence | Effect on Meta |
|--------|---------|----------------|
| Challenge Mode modifiers | Weekly | Each modifier buffs/nerfs specific combat axes. A "reduced healing" week punishes sustain builds; a "bonus crit damage" week rewards glass cannons |
| Solo Trial rotation | Weekly | Tests different axes (burst, sustain, mobility) — shifts which builds top leaderboards |
| Mythic dungeon rotation | Weekly | Each Mythic has different element ecology — domain loadouts that excel in one Mythic may struggle in the next |
| Dungeon drop table rotation | Weekly | Shifts farming targets — players chase different gems each week, creating natural build experimentation |
| Mob element composition | Weekly per dungeon | Storm-heavy trash one week, Void-heavy the next — builds must adapt defensively |

**Design intent:** At any given week, the "best" build for PvE is different from last week. Players who lock into one loadout fall behind players who adapt. This is the primary meta rotation engine and requires zero developer intervention once configured.

### Tier 2: Seasonal Modifiers (Quarterly, Pre-Planned)

Larger shifts applied at season boundaries that reshape the meta landscape.

| Modifier Type | Example | Duration |
|---------------|---------|----------|
| Resonance availability | Enable/disable specific conditional resonances per season | 3 months |
| Domain amplification | One domain gets +10% effectiveness in all content for the season | 3 months |
| Alignment season | One alignment gets a seasonal exclusive gem or bonus | 3 months |
| Environmental shift | New environmental hazard type added to all dungeons | 3 months |
| Modifier gem rotation | 2-3 new modifier gems introduced, 2-3 rotated to "legacy" (still usable, no longer drops — must craft) | 3 months |

**Seasonal resonance rotation** is the most powerful lever. Since resonances are discovered through play, enabling new conditional resonances each season creates a fresh discovery period where the community races to find new synergies. This keeps theorycrafting alive indefinitely.

### Tier 3: Data-Driven Balance Patches (Monthly)

Direct number adjustments based on live data. These are reactive — they fix problems the rotation systems didn't prevent.

**Patch cadence:** Monthly, published 1 week before deployment.

**Trigger thresholds:**

| Metric | Healthy | Watch List | Intervention |
|--------|---------|------------|--------------|
| PvP win rate (any loadout vs field) | 40-55% | 55-60% | >60% for 2+ weeks |
| PvP pick rate (any loadout) | <15% | 15-25% | >25% (over-centralizing) |
| PvE clear time variance | Within 30% across builds | 30-40% variance | >40% variance |
| Solo Trial top-10 archetype diversity | 5+ distinct archetypes | 3-4 archetypes | <3 archetypes |
| Domain pick rate (equipped gems) | 10-15% per domain (8 domains) | 5-10% or 15-25% | <5% or >25% |
| Modifier gem pick rate | Spread across categories | 3+ modifiers with <2% usage | Any modifier with >40% usage |
| Alignment distribution | 25-40% each | 20-25% or 40-50% | <20% or >50% |

**Intervention toolkit (lightest to heaviest):**

| Action | When to Use | Example |
|--------|-------------|---------|
| Coefficient adjustment | Win rate 55-60% | Reduce a domain's systemic rule intensity by 5-10% |
| PvP-specific coefficient | PvP imbalanced but PvE fine | Give a skill separate PvP scaling (already supported per [[PvP Structure]]) |
| Modifier value tuning | One modifier over-centralizing | Reduce Combo Finisher from +35% to +28% |
| Resonance adjustment | A resonance is too reliable | Increase the conditional difficulty or reduce the effect magnitude |
| Cooldown/cast time shift | A rotation is too fast/slow | Adjust by 0.5-1.0s |
| Stat weight redistribution | A domain's stat profile creates degenerate stacking | Shift 2-3 stat points between stats within the domain |

**What we never do:**
- Remove a gem, domain, or resonance entirely
- Reduce a value by more than 20% in a single patch
- Change multiple aspects of a single build simultaneously (one lever at a time)
- Apply PvP changes that affect PvE without separate review

### Tier 4: New Content Introduction (Per Expansion)

New gems, modifiers, and domains are the most disruptive meta events. They require structured rollout.

**New Gem Release Protocol:**

1. **Internal modeling** — Run new gem through [[Gem System Math]] DPS model. Verify power budget falls within 80-100 range for all intended loadouts.
2. **Resonance audit** — Map every new resonance the gem creates with existing domains. Flag any passive resonance that exceeds 15% effective DPS bonus.
3. **Modifier compatibility check** — Verify no modifier + new gem combination exceeds existing top builds by more than 10%.
4. **PTR period** — 2 weeks on public test realm. Invite top creators (per [[Creator Ecosystem]]) to stress-test.
5. **Staged release** — New gems enter the game through dungeon drops and crafting recipes. No direct purchase. This gates adoption speed and allows observation.
6. **30-day review** — Mandatory balance review 30 days post-release with full data analysis.

**New Modifier Release Protocol:**

Same as above but with additional check: verify the modifier doesn't create a new cross-category stacking breakpoint that obsoletes existing modifier choices.

### Tier 5: Emergency Intervention (Hotfix)

Reserved for game-breaking situations only.

**Triggers:**
- Any build exceeding 70% win rate
- Any exploit enabling infinite resources, stability, or resonance triggers
- Any build combination that trivializes Mythic content (clear time <50% of intended)
- Any degenerate interaction that removes counterplay entirely

**Protocol:**
1. Identify the specific interaction causing the problem
2. Apply the minimum change to break the degenerate case (disable one resonance, cap one value, add one cooldown)
3. Hotfix within 48 hours
4. Communicate clearly: what happened, what we changed, why, and what the full fix will look like in the next monthly patch
5. Compensate affected players if the fix invalidates significant investment (refund mastery XP, return materials)

---

## Counter-Matchup Framework

Every loadout should have clear strengths and weaknesses. The target is rock-paper-scissors at the domain level, not perfect balance.

### Domain Interaction Matrix

| Attacker \ Defender | Kinetics | Entropy | Resonance | Void | Flux | Tempest | Aegis | Synthesis |
|---------------------|----------|---------|-----------|------|------|---------|-------|-----------|
| **Kinetics** | -- | Weak | Neutral | Neutral | Weak | Strong | Neutral | Strong |
| **Entropy** | Strong | -- | Strong | Neutral | Neutral | Neutral | Weak | Weak |
| **Resonance** | Neutral | Weak | -- | Weak | Strong | Neutral | Strong | Neutral |
| **Void** | Neutral | Neutral | Strong | -- | Neutral | Weak | Neutral | Strong |
| **Flux** | Strong | Neutral | Weak | Neutral | -- | Strong | Weak | Neutral |
| **Tempest** | Weak | Neutral | Neutral | Strong | Weak | -- | Neutral | Strong |
| **Aegis** | Neutral | Strong | Weak | Neutral | Strong | Neutral | -- | Weak |
| **Synthesis** | Weak | Strong | Neutral | Weak | Neutral | Weak | Strong | -- |

**Reading this matrix:**
- **Strong:** Attacker's systemic rule naturally counters defender's strategy. Kinetics displacement disrupts Synthesis constructs. Entropy debuffs strip Aegis Counter-Energy efficiency.
- **Weak:** Attacker's systemic rule is naturally countered. Tempest chains are disrupted by Void null zones. Flux transformations are dampened by Resonance echoes.
- **Neutral:** No inherent advantage. Outcome depends on modifier choices, mutations, and player skill.

**With 4-domain loadouts**, every build contains a mix of strong and weak matchups. A loadout of Kinetics + Tempest + Entropy + Void is strong against Synthesis-heavy builds but weak against Flux + Aegis combinations.

### Target Matchup Distribution Per Loadout

| Matchup Type | Target Count (of 69 opposing loadouts) |
|--------------|----------------------------------------|
| Favored (>55% expected win rate) | 15-25 |
| Neutral (45-55%) | 25-40 |
| Unfavored (<45%) | 15-25 |

No loadout should have fewer than 10 unfavored matchups. No loadout should have more than 30 favored matchups.

---

## Mutation Management

Mutations are the wildcard. Locked-in mutations create unique gem variants that sit outside the standard balance framework because they weren't designed — they emerged.

### The Problem

A gem that has mutated through 50 fights may have an action profile that's significantly stronger (or weaker) than any designed gem. Because mutations are locked in permanently, standard balance patches that adjust base gem values don't touch mutated gems.

### Mutation Balance Rules

1. **Mutation power ceiling:** No single mutation can shift a gem's effective power by more than +25% from its base. The mutation system enforces this ceiling during the lock-in process. If a mutation would exceed this, the player is warned and the excess is trimmed.

2. **Mutation stat drift cap:** Mutated gem stat contributions cannot drift more than +/-30% from the domain's base stat profile. A Kinetics gem that mutates cannot become a better Acuity source than an Entropy gem.

3. **Seasonal mutation audit:** Each season, the system audits all locked-in mutations across the population. Mutations that appear on >5% of all gems of that domain are flagged for review — they indicate a dominant mutation path that may need its ceiling reduced.

4. **Gem maintenance fee:** Per [[Economy Stress Test]], gems with locked mutations require periodic NPC tuning (75 gold/gem/week). This creates a soft cost to maintaining extreme mutation libraries and acts as a gold sink.

5. **Mutation rebalance pass:** If a specific mutation variant is identified as degenerate (through data or community reporting), the dev team can issue a **mutation adjustment** — a one-time shift to the ceiling of that specific mutation type. Affected players receive a notification and the option to revert the mutation for free within 7 days.

6. **Mutation provenance tracking:** Every mutation's lineage (which fight, which stability threshold, which environmental conditions) is logged. This enables targeted rebalancing without blanket changes.

---

## Data Pipeline

Balance decisions require data. The following metrics are tracked in real-time and aggregated daily.

### Combat Metrics

| Metric | Granularity | Retention |
|--------|-------------|-----------|
| Win/loss per loadout (PvP) | Per match | 12 months |
| Win/loss per loadout pair (PvP) | Per match | 12 months |
| Clear time per dungeon per loadout | Per run | 12 months |
| Actions committed per round | Per round | 3 months |
| Momentum distribution per match | Per match | 6 months |
| Gem stability at fight end | Per fight | 3 months |
| Resonance trigger rate per pair | Per fight | 12 months |
| Mutation lock-in rate per domain | Per lock-in event | Permanent |
| Overcharge usage rate | Per round | 3 months |
| Desperation action success rate | Per use | 6 months |

### Economy Metrics (Meta-Relevant)

| Metric | Why It Matters for Meta |
|--------|------------------------|
| Gem domain trade volume | High volume = high demand = meta-relevant domain |
| Modifier gem price trends | Price spikes signal emerging meta builds |
| Crafting recipe popularity | Signals which builds players are investing in |
| Salvage rates by domain | High salvage = domain falling out of meta |

### Community Metrics

| Metric | Why It Matters for Meta |
|--------|------------------------|
| Published build archetype distribution | Shows where creator attention focuses |
| Build fork rate | High forks = active theorycrafting around a build |
| "Most Innovative" vote patterns | Signals community appetite for off-meta play |
| Forum/community sentiment per domain | Early warning for frustration or excitement |

### Monthly Meta Report (Public)

Published to all players at the start of each month:

- **Top 10 PvP loadouts** by pick rate and win rate
- **Top 10 PvE loadouts** by dungeon clear efficiency
- **Domain pick rate distribution** (pie chart)
- **Alignment distribution**
- **Rising builds** (biggest win rate or pick rate increase)
- **Falling builds** (biggest decreases)
- **Upcoming rotation preview** (next month's Challenge Mode themes, Mythic rotation, Solo Trial focus)

This report serves dual purposes: it gives players strategic information to plan builds, and it signals that the dev team is actively watching balance.

---

## Meta Health Indicators

| Indicator | Healthy | Unhealthy |
|-----------|---------|-----------|
| Number of competitively viable loadouts | 30-50+ | <20 |
| Highest single-loadout PvP pick rate | <15% | >25% |
| Domain pick rate spread | 8-18% per domain | Any domain <5% or >25% |
| Average PvP match duration | 8-15 rounds (2-5 min) | <5 rounds (burst meta) or >20 rounds (stall meta) |
| Counter-play availability | Every top-10 build has a known counter | Any top-10 build with no viable counter |
| Community sentiment | Active theorycrafting, build diversity celebrated | "Only one build works" complaints dominant |
| Seasonal meta shift | Top 5 loadouts change by 40%+ between seasons | Same top 5 for 2+ consecutive seasons |
| Mutation diversity | Mutations spread across many variants per domain | >50% of locked mutations converge on same path |

---

## Seasonal Meta Cycle

Each 3-month season follows a predictable rhythm:

### Weeks 1-2: Discovery Phase
- New seasonal modifiers active
- New resonances available for discovery
- Community races to find optimal combinations
- Meta is chaotic — many builds being tested
- **Dev posture:** Observe only. Do not intervene unless emergency.

### Weeks 3-4: Consolidation Phase
- Community converges on early meta favorites
- Counter-builds begin to emerge
- Creator Ecosystem publishes first wave of optimized builds
- **Dev posture:** Monitor. Begin tracking win rates for watch list.

### Weeks 5-8: Established Meta
- Meta stabilizes around 5-8 dominant archetypes
- Weekly rotations keep PvE meta shifting
- PvP meta is relatively stable
- **Dev posture:** Monthly patch if any metric exceeds intervention thresholds.

### Weeks 9-10: Late Meta
- Deep optimizations emerge
- Niche counter-builds find their audience
- Community anticipates next season's changes
- **Dev posture:** Prepare next season's modifiers based on current data. Identify which resonances to rotate.

### Weeks 11-12: Transition
- Season rewards distributed
- Ranked ladder soft-resets
- Preview of next season's changes published
- Off-season experimental mode active (wild modifiers, unrestricted rules)
- **Dev posture:** Deploy seasonal transition patch. New resonances, modifier rotations, and domain amplification changes go live.

---

## Anti-Stagnation Mechanisms

If the meta stalls despite rotation systems, escalating interventions:

| Stage | Trigger | Action |
|-------|---------|--------|
| 1 | Same top 3 builds for 4+ weeks | Increase Challenge Mode modifier intensity for the following week to specifically pressure dominant builds |
| 2 | Same top 3 for 6+ weeks | Mid-season resonance injection — enable 2-3 new conditional resonances that synergize with underplayed domains |
| 3 | Same top 3 for 8+ weeks | Direct balance patch targeting the specific interaction that makes these builds dominant |
| 4 | Same top 3 across 2 seasons | Systemic review — the problem is structural, not numerical. Revisit domain interaction matrix and consider domain rework |

---

## Related Pages

- [[Gem System Math]] -- Balance formulas and power budget
- [[Gem Identity System]] -- Domain definitions and resonance system
- [[Combat System]] -- Round structure, momentum, stability
- [[PvP Structure]] -- Competitive modes and seasonal framework
- [[PvE Structure]] -- Dungeon rotations and Challenge Mode
- [[Creator Ecosystem]] -- Community data and build publishing
- [[Economy Stress Test]] -- Mutation maintenance fees, economic health metrics
- [[Long-Term Vision]] -- Content expansion timeline
