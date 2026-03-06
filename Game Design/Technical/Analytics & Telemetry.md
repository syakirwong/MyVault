---
tags:
  - gdd
  - technical
  - analytics
  - telemetry
  - data
---

# Analytics & Telemetry

Defines the data infrastructure that supports the metrics referenced across [[MVP Design#MVP Success Metrics]], [[Meta Management#Data Pipeline]], and [[Economy Stress Test#Economic Health Dashboard]]. Those documents specify *what* to measure. This document specifies *how* the data flows, how it is stored, how it surfaces to the team, and how player privacy is protected.

---

## 1. Design Philosophy

### Measure What Matters, Not Everything

Every tracked event must answer a specific design question. Events that do not map to a dashboard, alert threshold, or balance decision are not collected.

| Principle | Implementation |
|-----------|----------------|
| **Design-driven collection** | Every event in the taxonomy below is tagged with the document and metric it serves. No orphan events. |
| **Signal over noise** | High-frequency, low-value events (e.g., every UI click, every panel open) are not tracked at the individual level. Aggregate interaction patterns are sufficient. |
| **Privacy by default** | Behavioral tracking serves game balance and player identity systems per [[Behavioral Identity]]. It does not serve advertising, does not profile players for external monetization, and does not share data with third parties. |
| **Data has a cost** | Storage, processing, and engineering attention are finite. Every new event must justify its ongoing cost against the insight it provides. Quarterly review of event utility. |

### Data Serves Three Audiences

| Audience | Needs | Cadence |
|----------|-------|---------|
| **Game designers** | Balance decisions, meta health, economy tuning | Daily dashboards, weekly reports, monthly meta reviews |
| **Engineers** | Performance monitoring, error rates, capacity planning | Real-time alerts, hourly aggregates |
| **Players** | Transparency about game health, meta state | Monthly meta report per [[Meta Management#Monthly Meta Report (Public)]] |

---

## 2. Event Taxonomy

All client-generated events follow a structured schema:

```
GameEvent {
  event_id:       UUID
  event_type:     string        // category.action (e.g., "combat.match_end")
  player_id:      UUID          // anonymized in analytics warehouse
  session_id:     UUID
  timestamp:      ISO 8601
  server_region:  string
  payload:        object        // event-specific data
}
```

### Session Events

| Event | Payload | Serves |
|-------|---------|--------|
| `session.login` | `{ platform, client_version, screen_resolution, browser_or_client }` | DAU/MAU, platform distribution, minimum requirements validation |
| `session.logout` | `{ duration_seconds, last_activity_type }` | Session length distribution per [[MVP Design]] (target: 25-40 min) |
| `session.heartbeat` | `{ current_context: hub/combat/menu, region_id }` | Concurrent player counts, content distribution, region population |
| `session.disconnect` | `{ context, reconnected: boolean, reconnect_time_ms }` | Connection stability monitoring, reconnection UX effectiveness |

### Combat Events

| Event | Payload | Serves |
|-------|---------|--------|
| `combat.match_start` | `{ match_type: pvp_1v1/pvp_3v3/pve_dungeon/pve_world_boss, loadout_hash, gem_domains[], alignment, behavioral_archetype }` | Match type distribution, loadout tracking per [[MVP Design]] |
| `combat.match_end` | `{ result: win/loss/draw/forfeit, rounds_played, duration_seconds, loadout_hash, opponent_loadout_hash, momentum_peaks[], final_integrity, gems_broken_count }` | Win rate analysis per [[Meta Management#Trigger Thresholds]], match duration health |
| `combat.round_resolved` | `{ round_number, actions_committed[], slot3_conditional_used: boolean, slot3_conditional_success: boolean, overcharge_used: boolean, damage_dealt, damage_taken, momentum_change }` | Round-level balance data, conditional action success rates, overcharge usage |
| `combat.resonance_triggered` | `{ resonance_id, gem_pair, trigger_context, effect_magnitude }` | Resonance discovery tracking per [[MVP Design]] (target: 50%+ found in month 1), resonance balance |
| `combat.gem_mutation` | `{ gem_id, mutation_type, stability_at_mutation, locked_or_reverted, domain }` | Mutation management per [[Meta Management#Mutation Management]], mutation affinity tracking for [[Behavioral Identity]] |
| `combat.gem_broken` | `{ gem_id, domain, rarity, stability_at_break, insured: boolean, dungeon_id }` | Gem breakage rate monitoring per [[Economy Stress Test#Scenario 5]] (target: 5-15%) |
| `combat.momentum_peak` | `{ peak_value, round_at_peak, desperation_action_triggered: boolean }` | Momentum distribution per [[Meta Management#Combat Metrics]] |

### Economy Events

| Event | Payload | Serves |
|-------|---------|--------|
| `economy.gold_earned` | `{ amount, source: dungeon/quest/salvage/marketplace_sale/npc_vendor, context_id }` | Gold supply tracking per [[Economy Stress Test#Scenario 4]] |
| `economy.gold_spent` | `{ amount, sink: marketplace_purchase/crafting_fee/insurance/fast_travel/storage/guild_upkeep/gem_maintenance/reforging/commission_fee/prestige, context_id }` | Sink effectiveness per [[Economy Stress Test#Scenario 4]] (daily sink target: ~4,289,000 gold at 10K players) |
| `economy.marketplace_listed` | `{ gem_id, domain, rarity, listed_price, appraisal_price, spread_fee_pct }` | Marketplace volume per [[Economy Stress Test#Economic Health Dashboard]] (target: >30,000/day) |
| `economy.marketplace_purchased` | `{ listing_id, sale_price, spread_fee, domain, rarity }` | Transaction velocity, NPC spread fee sink |
| `economy.marketplace_delisted` | `{ listing_id, delist_penalty }` | Delist penalty sink, manipulation monitoring per [[Economy Stress Test#Scenario 3]] |
| `economy.crafting_attempt` | `{ discipline, recipe_tier, materials_consumed, success: boolean }` | Crafting participation per [[MVP Design]] (target: 30%+) |
| `economy.crafting_result` | `{ discipline, output_rarity, output_domain, crafter_signature: boolean }` | Crafting output distribution, profitability tracking |
| `economy.shard_earned` | `{ amount, source: salvage/combat_drop/gold_conversion }` | Shard supply tracking per [[Economy Stress Test#Scenario 8]] |
| `economy.shard_spent` | `{ amount, sink: gem_shaping/catalyst_brewing/resonance_mapping/reforging }` | Shard sink tracking (target supply/demand ratio: 0.8-1.2) |
| `economy.resonance_token_earned` | `{ amount, source: boss_kill/resonance_discovery/dungeon_completion }` | Token earn rate per [[Economy Stress Test#Scenario 9]] (target: 3-4/day active endgame) |
| `economy.resonance_token_spent` | `{ amount, sink: legendary_craft/mutation_catalyst/resonance_catalyst/resonance_lock/resonance_journal }` | Token sink distribution |

### Progression Events

| Event | Payload | Serves |
|-------|---------|--------|
| `progression.level_up` | `{ new_level, hours_played_total, primary_content_type }` | Leveling curve health, content distribution at level milestones |
| `progression.gem_mastery_milestone` | `{ gem_id, domain, mastery_level, hours_with_gem }` | Gem experimentation per [[MVP Design]] (target: avg 5+ loadouts in first month) |
| `progression.alignment_change` | `{ old_alignment, new_alignment, old_depth, trigger_context }` | Alignment distribution per [[Meta Management#Trigger Thresholds]] (target: 25-40% each) |
| `progression.alignment_depth` | `{ alignment, new_depth, tier_name }` | Depth progression pacing |
| `progression.faction_reputation` | `{ faction_id, old_tier, new_tier }` | Faction engagement distribution |
| `progression.resonance_discovery` | `{ resonance_id, domain_pair, discovery_method, is_first_global: boolean }` | Community resonance discovery rate per [[MVP Design]] (target: 50%+ in month 1) |

### Social Events

| Event | Payload | Serves |
|-------|---------|--------|
| `social.friend_added` | `{ method: inspect/hub/build_card }` | Social feature adoption |
| `social.guild_joined` | `{ guild_size_at_join }` | Guild participation rates |
| `social.party_formed` | `{ party_size, content_target }` | Group content demand |
| `social.build_published` | `{ loadout_hash, gem_domains[] }` | Creator ecosystem engagement per [[MVP Design]] |
| `social.build_viewed` | `{ loadout_hash, viewer_relationship: friend/guild/stranger }` | Build sharing virality |
| `social.chat_volume` | `{ channel_type: region/party/guild/whisper, message_count }` | Chat health (volume only, NOT content) |

### Engagement Events

| Event | Payload | Serves |
|-------|---------|--------|
| `engagement.quest_started` | `{ quest_id, quest_type: main/side/daily_bounty }` | Quest participation |
| `engagement.quest_completed` | `{ quest_id, duration_seconds, rewards_earned }` | Quest completion funnels, reward pacing |
| `engagement.quest_abandoned` | `{ quest_id, progress_pct_at_abandon, reason: manual/timeout }` | Quest friction detection |
| `engagement.daily_bounty_claimed` | `{ bounty_tier, rewards }` | Daily engagement driver effectiveness |
| `engagement.content_played` | `{ content_type: pvp_arena/pvp_skirmish/dungeon_normal/dungeon_mythic/world_boss/crafting/marketplace/exploration, duration_seconds }` | Content breadth distribution per [[Behavioral Identity]] |
| `engagement.ui_response_time` | `{ action_type: commit_slot_select/action_bar_click/marketplace_search, response_ms }` | UI responsiveness per [[MVP Design]] (target: <2s avg response to enemy intent) |

---

## 3. Dashboard Requirements

### Health Dashboard

**Audience:** Full team. Reviewed daily.

| Panel | Metrics | Source Events | Thresholds |
|-------|---------|---------------|------------|
| Active Players | DAU, WAU, MAU, peak concurrent | `session.login`, `session.heartbeat` | DAU/MAU ratio < 0.15 = warning |
| Session Length | Distribution histogram, median, p25/p75 | `session.logout` | Median outside 25-40 min = investigate per [[MVP Design]] |
| Retention Cohorts | D1, D3, D7, D14, D30 by registration date | `session.login` vs registration | D30 < 40% = critical per [[MVP Design]] |
| Revenue (if applicable) | ARPU, ARPPU, conversion rate | Payment events (external system) | Monitor only at MVP |
| Platform Distribution | Browser vs desktop vs PWA, browser breakdown | `session.login` payload | Informational |
| Connection Stability | Disconnect rate, reconnection success rate, avg reconnect time | `session.disconnect` | Disconnect rate > 5% = investigate |

### Balance Dashboard

**Audience:** Game designers. Reviewed daily during live ops, weekly during stable periods.

| Panel | Metrics | Source Events | Thresholds (per [[Meta Management]]) |
|-------|---------|---------------|---------------------------------------|
| Win Rate by Loadout | Per-loadout win rate, filtered by PvP mode and MMR bracket | `combat.match_end` | Any loadout > 60% for 2+ weeks = intervention |
| Pick Rate by Loadout | Top 20 loadouts by pick rate | `combat.match_start` | Any loadout > 25% pick rate = over-centralizing |
| Domain Popularity | Pick rate per domain (equipped gems) | `combat.match_start` | Any domain < 5% or > 25% = warning per [[Meta Management]] |
| Resonance Discovery | Total discovered vs total available, discovery rate curve | `progression.resonance_discovery` | < 30% discovered in month 1 = tuning needed |
| DPS Distribution | Damage output per round by loadout, match duration distribution | `combat.round_resolved`, `combat.match_end` | Match duration < 5 rounds or > 20 rounds = meta problem |
| Archetype Diversity | Solo Trial top-10 archetype count | `combat.match_end` filtered by Solo Trial | < 3 archetypes = stagnation per [[Meta Management]] |
| Modifier Usage | Pick rate per modifier gem | `combat.match_start` | Any modifier > 40% usage = investigate |
| Momentum Profile | Average momentum by loadout, desperation action frequency | `combat.momentum_peak` | Informational — feeds [[Behavioral Identity]] calibration |

### Economy Dashboard

**Audience:** Game designers + economists. Reviewed daily.

| Panel | Metrics | Source Events | Thresholds (per [[Economy Stress Test]]) |
|-------|---------|---------------|------------------------------------------|
| Gold Supply | Total gold in circulation, daily net (sources - sinks), monthly growth rate | `economy.gold_earned`, `economy.gold_spent` | Growth > 3%/month = warning, > 5% = critical |
| Gold Sources | Breakdown by source category (dungeon, quest, salvage, marketplace, NPC) | `economy.gold_earned` | Any single source > 50% of total = over-concentrated |
| Gold Sinks | Breakdown by sink category | `economy.gold_spent` | Verify sink targets per [[Economy Stress Test#Scenario 4]] |
| Marketplace Velocity | Daily transactions, listings created, average time-to-sale | `economy.marketplace_listed`, `economy.marketplace_purchased` | Volume < 15,000/day = warning, < 30,000 = target per stress test |
| NPC Price Trends | Weighted average price by domain and rarity, 7/30/90-day charts | `economy.marketplace_purchased` | Common gem price outside 80-120 gold = warning |
| Crafting Participation | % of active players crafting, profitability margin | `economy.crafting_attempt`, `economy.crafting_result` | Participation < 30% or margin < 5% = critical |
| Shard Economy | Supply/demand ratio, source/sink breakdown, monthly growth | `economy.shard_earned`, `economy.shard_spent` | Ratio outside 0.8-1.2 = warning |
| Resonance Token Flow | Earn rate (per active endgame player/day), sink distribution | `economy.resonance_token_earned`, `economy.resonance_token_spent` | Earn rate < 2 or > 6/day = warning |
| Wealth Distribution | Gini coefficient, wealth percentile chart | Derived from player balance snapshots | Gini > 0.55 = warning, > 0.70 = critical |

### Content Dashboard

**Audience:** Game designers. Reviewed weekly.

| Panel | Metrics | Source Events |
|-------|---------|---------------|
| Dungeon Completion | Completion rate per dungeon, abandon rate, avg clear time | `combat.match_end` filtered by PvE |
| PvP Participation | Daily unique PvP players, queue times, match counts by mode | `combat.match_start` filtered by PvP |
| Quest Funnels | Start > progress > complete > abandon rates per quest | `engagement.quest_started`, `engagement.quest_completed`, `engagement.quest_abandoned` |
| Content Difficulty | Gem breakage rate per dungeon, insurance usage per dungeon | `combat.gem_broken`, `economy.gold_spent` (insurance sink) |
| New Player Funnel | Tutorial completion, time to first 4 gems, time to first marketplace purchase, time to first PvP match | Multiple events, filtered by account age < 30 days |
| Behavioral Identity | Archetype distribution across population, axis histogram | Derived from Identity Service data |

---

## 4. Alert Thresholds

Automated alerts trigger when metrics cross defined boundaries. Alerts route to the appropriate team channel.

### Balance Alerts

| Condition | Severity | Action | Source |
|-----------|----------|--------|--------|
| Any loadout > 60% win rate for 48+ hours | **High** | Flag for balance review. Add to watch list per [[Meta Management#Tier 3]]. | `combat.match_end` aggregate |
| Any loadout > 70% win rate at any point | **Critical** | Emergency intervention protocol per [[Meta Management#Tier 5]]. | `combat.match_end` aggregate |
| Any domain pick rate < 5% or > 25% for 7+ days | **Medium** | Review seasonal modifiers and rotation schedule. | `combat.match_start` aggregate |
| Solo Trial top-10 has < 3 distinct archetypes | **Medium** | Anti-stagnation Stage 1 per [[Meta Management#Anti-Stagnation Mechanisms]]. | `combat.match_end` aggregate |
| Average PvP match duration < 5 rounds or > 20 rounds | **High** | Burst meta or stall meta detected. Urgent balance review. | `combat.match_end` aggregate |

### Economy Alerts

| Condition | Severity | Action | Source |
|-----------|----------|--------|--------|
| Gold supply growth > 3%/month | **High** | Review sink effectiveness. Consider emergency levers per [[Economy Stress Test#Scenario 4]]. | `economy.gold_earned/spent` aggregate |
| Gold supply growth > 5%/month | **Critical** | Activate emergency levers. | Same |
| Shard supply/demand ratio outside 0.5-1.5 | **High** | Adjust salvage yields or crafting costs per [[Economy Stress Test#Scenario 8]]. | `economy.shard_earned/spent` aggregate |
| Gem breakage rate outside 3-25% (high-risk dungeons) | **High** | Review stability drain rates. | `combat.gem_broken` aggregate |
| Account net worth increases > 300% in 24 hours | **Critical** | Potential duplication exploit per [[Economy Stress Test#Scenario 2]]. Investigate immediately. | Derived from player balance snapshots |
| Any gem ID appears twice in the system | **Critical** | Duplication exploit confirmed. Freeze affected accounts. | Gem table integrity check |
| Marketplace daily volume < 15,000 | **Medium** | Economy engagement declining. Review listing caps and spread fees. | `economy.marketplace_purchased` count |

### Retention Alerts

| Condition | Severity | Action | Source |
|-----------|----------|--------|--------|
| D1 retention drops > 10% week-over-week | **Critical** | Investigate onboarding, recent changes, server stability. | Cohort analysis |
| D7 retention drops > 10% week-over-week | **High** | Review content engagement, economy health, balance state. | Cohort analysis |
| D30 retention below 30% (sustained) | **Critical** | Systemic review of core loop engagement. | Cohort analysis |
| Average session length drops below 15 minutes (sustained) | **High** | Content or engagement issue. Review quest completion, combat satisfaction. | `session.logout` aggregate |

### Infrastructure Alerts

| Condition | Severity | Action | Source |
|-----------|----------|--------|--------|
| Combat resolver latency p99 > 500ms | **Critical** | Scale combat resolver pool. Investigate hot path. Per [[Technical Architecture#Performance Targets]]. | Application metrics |
| Marketplace transaction failure rate > 1% | **High** | Database investigation. Connection pool review. | Application metrics |
| WebSocket error rate > 5% | **High** | Gateway scaling or network issue. | Application metrics |
| Disconnect rate > 5% of active sessions | **Medium** | Network infrastructure review. | `session.disconnect` aggregate |

---

## 5. A/B Testing Framework

### Capability

The analytics infrastructure supports controlled experiments to make data-driven design decisions.

| Component | Implementation |
|-----------|----------------|
| Feature flags | Server-side feature flag system. Players are assigned to experiment groups on login. Assignment is sticky (same player stays in same group across sessions). |
| Group assignment | Random assignment with stratification by: player level, account age, region, and behavioral archetype. Ensures balanced groups. |
| Minimum sample | No experiment concludes with fewer than 1,000 players per group or 7 days of data, whichever comes later. |
| Statistical rigor | Two-tailed t-test for continuous metrics (session length, gold earned). Chi-squared for categorical metrics (retention yes/no). p < 0.05 required for significance. |
| Mutual exclusion | A player can be in at most 2 concurrent experiments. Experiments that modify the same system are mutually exclusive. |

### Experiment Categories

| Category | Example Experiments | Guardrail Metrics |
|----------|--------------------|--------------------|
| **Balance parameters** | Test +5% Kinetics damage vs control. Measure win rate delta, pick rate change, match duration. | Win rate must not exceed 60% in test group. |
| **UI variations** | Test combat log font size increase. Measure UI response time, session length, combat log scrollback usage. | No degradation in commit-phase response time. |
| **Quest reward values** | Test +20% gold from daily bounties. Measure daily bounty claim rate, session length, gold supply impact. | Gold supply growth must stay < 3%/month. |
| **Economy tuning** | Test reduced NPC spread fee (10% vs 12% at lowest tier). Measure marketplace volume, sink effectiveness, new player purchasing power. | Gold supply growth must stay < 3%/month. |
| **Onboarding flow** | Test accelerated tutorial (skip origin selection, default to Ironsworn). Measure D1 retention, time to first PvP match, build experimentation rate. | D7 retention must not decrease. |

### Gradual Rollout

Feature flags also support non-experimental gradual rollouts:

| Phase | Audience | Duration |
|-------|----------|----------|
| Internal | Dev team + QA | Until stable |
| Canary | 1% of players (random) | 24-48 hours |
| Limited | 10% of players | 3-7 days |
| General | 50% -> 100% | Ramp over 2-3 days |

Rollback at any phase if guardrail metrics are violated.

---

## 6. Privacy Compliance

### GDPR & Data Protection

| Requirement | Implementation |
|-------------|----------------|
| **Consent** | First login displays a clear, non-coercive consent dialog. Players choose: (1) Accept gameplay analytics (required for core systems like [[Behavioral Identity]]), (2) Accept optional engagement analytics, (3) Reject optional analytics. Rejecting optional analytics does not degrade gameplay. |
| **Data minimization** | Only events listed in the taxonomy above are collected. No device fingerprinting, no IP geolocation beyond server region, no third-party tracking pixels. |
| **Anonymization** | In the analytics data warehouse, `player_id` is replaced with a one-way hash. Raw player IDs exist only in the operational database. Analytics queries cannot reverse-map to individual accounts without a separate, access-controlled lookup. |
| **Right to access** | Players can request a full export of their data via account settings. Export includes: behavioral signature, combat history summary (aggregated, not raw logs), economy transaction history, social connections, and all settings. |
| **Right to erasure** | Players can request account deletion. Operational data is deleted within 30 days. Anonymized analytics data is retained (cannot be re-identified). Gem provenance records are anonymized (owner IDs replaced with "deleted_account" placeholder to preserve marketplace integrity). |
| **Data retention** | See retention limits below. |
| **Data portability** | Account export in machine-readable JSON format. |

### What Is Never Tracked

| Data | Policy | Reasoning |
|------|--------|-----------|
| Chat message content | **Never collected.** Only message counts per channel type. | Player communication is private. Volume metrics suffice for chat health. |
| Real-world identity | **Never linked.** Account is email + password or OAuth. No name, address, phone number required. | Not relevant to game operation. |
| IP address | **Not stored in analytics.** Used transiently for rate limiting and regional routing. Discarded after session. | Server region tag provides sufficient geographic data. |
| Payment details | **Handled by third-party payment processor.** Never touches game servers. | PCI compliance. Not our data. |
| Device fingerprint | **Not collected.** Browser/client version string is the extent of device data. | Anti-cheat relies on server-side validation per [[Technical Architecture#Security Layer]], not client fingerprinting. |

### Chat Privacy

Per [[Chat & Communication System]], chat is a player communication tool. The analytics system tracks:

- Messages sent per channel type (region/party/guild/whisper) per session -- **count only**
- No message content, no keywords, no sentiment analysis, no moderation-through-analytics

Chat moderation is a separate system with its own policies, not a function of the analytics pipeline.

---

## 7. Data Pipeline

### Architecture

```
                     ┌──────────────┐
                     │  Game Client  │
                     └──────┬───────┘
                            │
                     ┌──────▼───────┐
                     │  Gateway API  │──── Authentication, rate limiting
                     └──────┬───────┘
                            │
              ┌─────────────▼──────────────┐
              │     Event Collector API     │──── Schema validation, enrichment
              │   (lightweight HTTP/batch)  │     (adds server timestamp, region)
              └─────────────┬──────────────┘
                            │
              ┌─────────────▼──────────────┐
              │     Message Queue (NATS)    │──── Buffering, fan-out
              │                            │
              └──┬──────────────────────┬──┘
                 │                      │
    ┌────────────▼───────┐   ┌─────────▼──────────┐
    │  Stream Processor  │   │   Batch Processor   │
    │   (Real-Time)      │   │   (Hourly/Daily)    │
    │                    │   │                      │
    │  - Alert evaluation│   │  - Aggregation       │
    │  - Live dashboards │   │  - Cohort analysis   │
    │  - Anomaly detect  │   │  - A/B test analysis │
    └────────┬───────────┘   └─────────┬────────────┘
             │                         │
    ┌────────▼─────────────────────────▼────────┐
    │              Data Warehouse                │
    │          (TimescaleDB / ClickHouse)        │
    │                                            │
    │  Raw events (retention-limited)            │
    │  Aggregated metrics (long-term)            │
    │  Cohort snapshots                          │
    └────────────────────┬──────────────────────┘
                         │
              ┌──────────▼──────────┐
              │   Dashboard Layer   │
              │     (Grafana)       │
              └─────────────────────┘
```

### Pipeline Components

| Component | Responsibility | Technology Recommendation |
|-----------|---------------|---------------------------|
| Event Collector | Receives events from clients via the Gateway API. Validates schema. Adds server-side timestamp and region tag. Batches for efficiency. | Lightweight HTTP service. Clients batch events locally and flush every 5 seconds or on 20 accumulated events. |
| Message Queue | Buffers events between collection and processing. Provides fan-out to real-time and batch consumers. | NATS JetStream (already in the architecture per [[Technical Architecture#Event-Driven Architecture]]). Reuse existing infrastructure. |
| Stream Processor | Evaluates real-time alert conditions. Feeds live dashboard panels. Detects anomalies (sudden spikes in gold generation, abnormal win rate shifts). | Lightweight consumer service. No heavy framework needed at MVP scale. At growth, consider Apache Flink or Materialize. |
| Batch Processor | Hourly and daily aggregation jobs. Cohort retention analysis. A/B test statistical evaluation. Economy supply/demand calculations. | Scheduled jobs (cron or Kubernetes CronJobs). SQL-based aggregation against the warehouse. |
| Data Warehouse | Long-term storage of raw events (retention-limited) and aggregated metrics (indefinite). Powers dashboards and ad-hoc queries. | TimescaleDB (already in the stack per [[Technical Architecture#Database Architecture]]) for time-series metrics. Consider ClickHouse for high-cardinality analytics queries at scale. |
| Dashboard Layer | Visualization of all dashboards defined in Section 3. Alert configuration and routing. | Grafana (already referenced in [[Technical Architecture#Monitoring & Alerting]]). |

### Event Volume Estimates

| Scale | Concurrent Players | Estimated Events/Second | Daily Event Volume |
|-------|-------------------|------------------------|--------------------|
| MVP (1,000 players) | 1,000 | ~200-400 | ~20-35 million |
| Growth (10,000 players) | 10,000 | ~2,000-4,000 | ~200-350 million |
| Scale (50,000 players) | 50,000 | ~10,000-20,000 | ~1-1.7 billion |

**Calculation basis:** An active combat session generates ~15-25 events per round (round resolved, momentum, resonance triggers, gem stability updates, gold/shard changes). With 8-15 rounds per match and ~200 concurrent sessions at MVP, combat alone generates ~100-200 events/second. Hub, economy, and progression events add ~50-100% overhead.

### Data Retention

| Data Type | Retention | Reasoning |
|-----------|-----------|-----------|
| Raw session events | 90 days | Short-term debugging and investigation. Aggregates serve long-term needs. |
| Raw combat events | Per [[Meta Management#Combat Metrics]]: 3-12 months by metric | Granular combat data drives balance decisions. Mutation lock-in data is permanent. |
| Raw economy events | 12 months | Economy stress test validation requires full annual cycle. |
| Aggregated metrics (daily/hourly) | Indefinite | Small data volume. Historical trends are permanently valuable. |
| Cohort retention data | Indefinite | Player lifetime value analysis. Small data volume. |
| A/B test results | Indefinite | Decision records. Small data volume. |
| Alert history | 24 months | Post-mortem analysis. |

### Real-Time vs Batch

| Metric Type | Processing Mode | Latency |
|-------------|----------------|---------|
| Alert conditions (balance, economy, infrastructure) | **Real-time** | < 60 seconds from event to alert |
| Live dashboard panels (concurrent players, active matches) | **Real-time** | < 30 seconds |
| Win rate aggregation by loadout | **Near-real-time** | 5-minute rolling window |
| Daily retention, economy supply/demand | **Batch** | Hourly refresh |
| Cohort analysis, A/B test evaluation | **Batch** | Daily refresh |
| Monthly meta report | **Batch** | Monthly manual trigger + review |

---

## 8. Integration Points

### Cross-Document Metric Alignment

This table maps every externally-referenced metric to its source event and dashboard.

| Metric | Referenced In | Source Event | Dashboard |
|--------|--------------|--------------|-----------|
| Session length 25-40 min | [[MVP Design#MVP Success Metrics]] | `session.logout` | Health: Session Length |
| Build diversity 30+ viable loadouts | [[MVP Design#MVP Success Metrics]] | `combat.match_end` | Balance: Win Rate by Loadout |
| Avg 5+ loadouts in first month | [[MVP Design#MVP Success Metrics]] | `progression.gem_mastery_milestone` | Content: New Player Funnel |
| 50%+ resonances found in month 1 | [[MVP Design#MVP Success Metrics]] | `progression.resonance_discovery` | Balance: Resonance Discovery |
| 30%+ crafting participation | [[MVP Design#MVP Success Metrics]] | `economy.crafting_attempt` | Economy: Crafting Participation |
| 40%+ D30 retention | [[MVP Design#MVP Success Metrics]] | `session.login` cohort | Health: Retention Cohorts |
| 50%+ marketplace listing | [[MVP Design#MVP Success Metrics]] | `economy.marketplace_listed` | Economy: Marketplace Velocity |
| Gold supply growth < 3%/month | [[MVP Design#MVP Success Metrics]], [[Economy Stress Test]] | `economy.gold_earned/spent` | Economy: Gold Supply |
| <2s UI response to enemy intent | [[MVP Design#MVP Success Metrics]] | `engagement.ui_response_time` | Content: UI Responsiveness |
| Win rate per loadout | [[Meta Management#Combat Metrics]] | `combat.match_end` | Balance: Win Rate by Loadout |
| Domain pick rate | [[Meta Management#Trigger Thresholds]] | `combat.match_start` | Balance: Domain Popularity |
| Gem breakage rate 5-15% | [[Economy Stress Test#Scenario 5]] | `combat.gem_broken` | Economy: Content Difficulty |
| Gini coefficient < 0.55 | [[Economy Stress Test#Economic Health Dashboard]] | Player balance snapshots | Economy: Wealth Distribution |

---

## Related Pages

- [[Technical Architecture]] -- Infrastructure, data stores, monitoring stack
- [[Meta Management]] -- Balance metrics, intervention thresholds, data pipeline requirements
- [[Economy Stress Test]] -- Economic health dashboard, supply/demand targets, alert thresholds
- [[MVP Design]] -- Success metrics that this system measures
- [[Behavioral Identity]] -- Behavioral axis tracking that feeds into analytics
- [[GDD Hub]] -- Master document index
