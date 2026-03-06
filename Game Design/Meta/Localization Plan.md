---
tags:
  - gdd
  - meta
  - localization
  - i18n
---

# Localization Plan

This document defines the localization strategy for Stormbound Online. Localization is not translation. It is the process of preserving mechanical clarity, tonal identity, and cultural resonance across every supported language — in a game where the written word carries gameplay weight.

For combat log narrative language, see [[Combat Logs System]].
For alignment-variant NPC dialogue, see [[NPC System]].
For behavioral combat log voice variants, see [[Behavioral Identity]].
For gem domain naming and archetype generation, see [[Gem Identity System]].

---

## Localization Philosophy

### Core Principles

1. **Mechanical clarity is non-negotiable.** Combat log flavor text encodes gameplay information — mutation thresholds, resonance proximity, overcharge signals. A translated string that loses its mechanical signal is a broken string. Every narrative template must preserve the decodable pattern in the target language.

2. **Tone is identity.** Stormbound Online has three alignment voices (Radiant, Corrupted, Unbound) and 10+ behavioral archetype voices. These tonal distinctions must survive translation. A Radiant NPC's warmth, a Corrupted NPC's edge, and an Unbound NPC's detachment are not decorative — they signal alignment to the player.

3. **Cultural adaptation over literal translation.** Idioms, metaphors, and naming conventions must be adapted, not transliterated. A gem archetype name like "Chaosweaver" needs a culturally resonant equivalent, not a compound word that sounds awkward in the target language.

4. **Consistency is learnable.** Players learn to read the combat log's narrative signals. If "The storm's edges crackle with unfamiliar rhythm" always means a Tempest gem entering Shifting stability, the translated equivalent must also be a fixed, consistent phrase. Variation in translation where the source is fixed breaks the learning loop.

5. **Context is king.** Every string sent for translation includes its mechanical context: where it appears, what it signals, what constraints it has (character length, tone, alignment variant).

---

## Text Volume Estimate

Approximate string counts by category for MVP scope (2 regions, 3 dungeons, 8 domains, 3 alignments).

### UI & System Strings

| Category | Estimated Strings | Notes |
|----------|-------------------|-------|
| UI labels (menus, buttons, headers) | 300-400 | Hub panel, combat panel, build panel, settings |
| System messages (errors, warnings, confirmations) | 150-200 | Connection errors, validation, session timeout |
| Tutorial text | 100-150 | Onboarding flow, tooltips, contextual help |
| Settings & controls labels | 50-80 | Accessibility options, keybinds, display settings |
| Notification & mail templates | 40-60 | System mail, achievement alerts, trade confirmations |

**Subtotal: ~640-890 strings**

### Combat Strings

| Category | Estimated Strings | Notes |
|----------|-------------------|-------|
| Combat action names | 24-32 | 8 domains x 3-4 actions each |
| Combat action descriptions | 24-32 | Tooltip text per action |
| Combat log templates (base) | 80-120 | Slot resolution, damage dealt/taken, buff/debuff applied, round summary |
| Combat log voice variants | 480-720 | Base templates x 6 archetype voices (Predator, Sentinel, Wildcard, Tactician, Architect, Phantom) — MVP ships 3 voices |
| Mutation flavor text | 40-50 | 8 domains x Shifting/Unstable/Critical/Broken thresholds + generic variants |
| Resonance signal text | 30-40 | Proximity signals (3/2/1/trigger) x self/opponent variants |
| Overcharge signal text | 10-15 | Self and opponent overcharge narration |
| Counter Prompt text | 20-30 | Pattern detection templates |
| Boss mechanic log lines | 15-25 | Per boss phase transitions and unique mechanics |
| Post-combat summary templates | 20-30 | Victory/defeat, stat summaries, mutation availability |
| PvE tactical hints | 20-30 | PvE-specific counter prompts and boss telegraphs |

**Subtotal: ~760-1,125 strings**

### Combat Log Voice Variants — Detail

The combat log voice system (see [[Behavioral Identity#Layer 2 Combat Log Voice]]) multiplies string counts significantly. Each base combat log template has up to 6 archetype-specific rewrites.

| Voice Archetype | Templates at MVP | Status |
|-----------------|------------------|--------|
| Predator | ~80 | MVP (launch) |
| Sentinel | ~80 | MVP (launch) |
| Tactician | ~80 | MVP (launch) |
| Wildcard | ~80 | Post-MVP |
| Architect | ~80 | Post-MVP |
| Phantom | ~80 | Post-MVP |
| Default (plain) | ~80 | MVP (launch, always available) |

**MVP voice strings: ~320. Full voice strings: ~560.**

### NPC & Narrative Strings

| Category | Estimated Strings | Notes |
|----------|-------------------|-------|
| Story NPC dialogue (alignment variants) | 600-900 | ~5 story NPCs x ~40-60 dialogue lines x 3 alignment variants |
| Service NPC dialogue (alignment variants) | 300-450 | ~10 service NPCs x ~10-15 lines x 3 alignment variants |
| Ambient NPC lines | 150-250 | ~25 ambient NPCs x 6-10 context-reactive lines |
| Quest text (objectives, descriptions, completion) | 200-300 | ~30-40 quests x 5-8 text entries each |
| Lore/codex entries | 100-150 | World lore, domain descriptions, faction history |
| Crafting NPC dialogue (recipe teaching, commentary) | 80-120 | 4 crafting masters x 20-30 contextual lines |

**Subtotal: ~1,430-2,170 strings**

### Item & Economy Strings

| Category | Estimated Strings | Notes |
|----------|-------------------|-------|
| Gem names and descriptions | 80-120 | Per domain per rarity tier |
| Modifier gem names and descriptions | 40-60 | MVP modifier pool |
| Crafting recipe names and descriptions | 60-80 | 4 disciplines, MVP recipes |
| Currency names and descriptions | 10-15 | Gold, Shards, Resonance Tokens, etc. |
| Marketplace UI text | 30-40 | Listing, bidding, appraisal, history |
| Archetype names (generated) | 70-100 | Base archetypes + playstyle modifiers + resonance titles |

**Subtotal: ~290-415 strings**

### Total MVP Estimate

| Category | Low | High |
|----------|-----|------|
| UI & System | 640 | 890 |
| Combat | 760 | 1,125 |
| NPC & Narrative | 1,430 | 2,170 |
| Items & Economy | 290 | 415 |
| **Total** | **3,120** | **4,600** |

**Post-MVP projection:** Each new region adds ~500-800 strings (NPCs, quests, ambient). Each new behavioral voice adds ~80 strings. Each new gem domain adds ~30-40 strings. Seasonal content adds ~100-200 strings per season.

---

## Priority Languages

### Phase 1: Launch

| Language | Rationale |
|----------|-----------|
| **English** | Source language. All content authored in English first. |

English-only launch reduces localization risk during the critical launch window. All string infrastructure is built during this phase — externalized resource files, context documentation, interpolation testing.

### Phase 2: Western European + Latin American (Month 3-6 Post-Launch)

| Language | Rationale |
|----------|-----------|
| **Spanish (Latin American)** | Largest non-English gaming market in the Americas. Strong MMO and RPG community. Text-based games have lower localization barrier than voice-acted titles. |
| **Brazilian Portuguese** | Massive and underserved gaming market. High engagement with online games. |
| **French** | Significant European market. Strong RPG tradition. |
| **German** | Major European market. RPG and strategy game affinity. Note: German text expansion (~30% longer than English) is the primary technical stress test. |

**Rationale for ordering:** These four languages cover the largest addressable Western markets for an online RPG. Text-heavy games benefit from literate, RPG-engaged audiences — all four qualify. Latin American Spanish and Brazilian Portuguese are prioritized over European Spanish/Portuguese due to market size and online gaming penetration.

### Phase 3: East Asian (Month 9-12 Post-Launch)

| Language | Rationale |
|----------|-----------|
| **Japanese** | Deep RPG culture. High tolerance for text-heavy games. Strong theorycraft community. |
| **Korean** | Dominant online gaming market. MMO-native audience. Competitive PvP culture aligns with Stormbound's arena system. |
| **Simplified Chinese** | Largest gaming market by player count. Requires additional regulatory compliance (content review, publishing partner). |

**Rationale for ordering:** East Asian localization requires CJK font support, significant cultural adaptation (naming conventions, humor, metaphor), and in China's case, regulatory and publishing infrastructure. This justifies the later phase.

### Future Consideration

| Language | Notes |
|----------|-------|
| Traditional Chinese | Taiwan/Hong Kong market. Lower priority than Simplified but shares font requirements. |
| Thai | Growing SEA market. Requires Thai script rendering. |
| Arabic | Requires RTL (right-to-left) UI layout support. Significant engineering investment. Defer until RTL framework is built. |
| Russian | Large PC gaming market. Cyrillic font support needed. |
| Italian | Smaller but engaged RPG market. Can share Phase 2 translation infrastructure. |

---

## Localization Challenges

### Challenge 1: Combat Log Narrative Signals

**The problem:** The combat log uses specific narrative phrases to encode hidden mechanical information (see [[Combat Logs System#Narrative Signal Language]]). Players learn to read phrases like "The storm's edges crackle with unfamiliar rhythm" as meaning "Tempest gem entering Shifting stability zone." This is a fixed mapping — the phrase always means the same thing.

**Why it's hard:** Translation naturally produces varied phrasings. A translator given the same English phrase twice might translate it differently each time. But in Stormbound, the player is learning a vocabulary. Inconsistency breaks the learning loop.

**Solution:**
- Every narrative signal string is tagged with a `FIXED_SIGNAL` flag in the string resource file.
- `FIXED_SIGNAL` strings include a mechanical context comment: "This phrase signals that a Tempest gem has crossed the 75-stability threshold into Shifting zone. The translated phrase must be: (a) distinct from all other mutation signals, (b) evocative of change/instability, (c) exactly the same every time this event occurs."
- Translators receive a Signal Glossary per language — a controlled vocabulary of narrative signal translations that are locked after approval.
- QA verifies signal consistency: every occurrence of a `FIXED_SIGNAL` in the target language must be byte-identical.

### Challenge 2: Alignment Tone Variants

**The problem:** NPC dialogue has 3 variants per alignment (see [[NPC System#NPC Dialogue Design]]). The tonal distinction is not just word choice — it is personality:

| Alignment | Tone | Characteristics |
|-----------|------|-----------------|
| Radiant | Warm, protective, ordered | Clean sentence structure, positive framing, references to light and potential |
| Corrupted | Sharp, charismatic, power-focused | Cryptic phrasing, philosophical edge, references to void and ambition |
| Unbound | Detached, measured, questioning | Balanced sentence structure, questions over statements, references to equilibrium |

**Why it's hard:** Tone is culturally embedded. What reads as "warm" in English may read as "formal" in Japanese. What reads as "sharp" in English may read as "rude" in Brazilian Portuguese where directness has different social weight.

**Solution:**
- Each alignment receives a Tone Guide per language — a document describing how the alignment's personality manifests in that language's cultural norms.
- Tone Guides are written by native-speaker localization consultants, not derived mechanically from the English.
- Example: Corrupted tone in Japanese might lean into keigo (formal speech) used ironically, rather than blunt directness.
- QA includes a tone consistency review where native speakers read all 3 alignment variants of the same NPC interaction and confirm the tonal distinction is perceptible.

### Challenge 3: Archetype Naming

**The problem:** The [[Gem Identity System#Emergent Archetype Naming]] system generates compound names like "Stormkinetic," "Chaosweaver," "Harmonic Bastion." The [[Behavioral Identity#Signature Archetypes]] system adds behavioral prefixes like "Predator," "Sentinel," "Phantom." These compound and layer:

```
[Corrupted] Proven Tactician Chaosweaver Assault Kael, Resonance Explorer
```

**Why it's hard:** Compound nouns work differently across languages. German creates them naturally (but they get long). Japanese uses different word order. Romance languages may need restructuring. The generated nature of the name means the translation system must handle *combinatorial* naming, not just static strings.

**Solution:**
- Archetype name components are translated as individual tokens with grammatical metadata (gender, number, case where applicable).
- Each language defines its own archetype name template: `{behavioral_prefix} {domain_archetype} {playstyle_modifier} {player_name}, {milestone_suffix}`.
- Template word order can differ per language.
- Some compound names may need to be adapted rather than directly translated. "Chaosweaver" in Japanese might become a two-kanji compound rather than a literal translation of "chaos" + "weaver."
- Archetype names are reviewed as a complete set per language to ensure distinctiveness — no two archetypes should sound similar in the target language.

### Challenge 4: Behavioral Combat Log Voice

**The problem:** The [[Behavioral Identity#Layer 2 Combat Log Voice]] system changes how the combat log narrates actions based on the player's behavioral archetype. The same action has 6+ narrative renderings:

| Archetype | Narration |
|-----------|-----------|
| Predator | "You lunge: Storm Strike tears through" |
| Sentinel | "You respond: Storm Strike answers" |
| Wildcard | "You gamble: Storm Strike — why not?" |

**Why it's hard:** Each voice is a distinct literary personality. Translating 6 voices means maintaining 6 distinct authorial tones per language. This is closer to creative writing than translation.

**Solution:**
- Voice variants are treated as creative localization, not technical translation.
- Each language hires a dedicated writer (not just translator) for each behavioral voice.
- Writers receive the English voice as reference plus a Voice Profile document describing the personality in language-agnostic terms (e.g., "Predator voice: short sentences, active verbs, no hesitation, treats combat as consumption").
- Writers produce target-language voice variants that capture the personality natively, not as translations of the English phrasing.
- Minimum 3 voices at MVP launch per language. Remaining voices added post-launch.

### Challenge 5: Dynamic String Interpolation

**The problem:** Combat log templates contain interpolated variables — damage numbers, player names, gem names, domain names, round numbers:

```
[R{round}] {player}: {action} -> {target} ({damage} {domain} damage)
```

**Why it's hard:** Word order varies by language. In English, "dealt 299 Tempest damage" is natural. In Japanese, the damage number and domain might need to appear in different positions. Gendered languages may need the domain name to agree grammatically with surrounding text.

**Solution:**
- All interpolated strings use named placeholders, not positional: `{damage}`, `{domain}`, `{target}`, not `%1`, `%2`, `%3`.
- Translators can reorder placeholders freely within the template.
- Domain names carry grammatical metadata per language (gender, article requirements).
- String interpolation engine supports pluralization rules per language (English: 1 round/2 rounds; Polish: 1 runda/2 rundy/5 rund).
- All interpolated strings are tested with edge-case values: maximum damage numbers (5+ digits), long player names (20+ characters), all 8 domain names.

---

## Technical Requirements

### String Externalization

- **All** player-facing text lives in structured resource files (JSON or YAML), never hardcoded.
- Resource files are organized by system: `ui.json`, `combat.json`, `npcs/{npc_name}.json`, `quests/{quest_id}.json`, `items.json`.
- Each string entry includes:
  - `key`: Unique identifier (e.g., `combat.mutation.tempest.shifting`)
  - `value`: English source text
  - `context`: Where it appears, what it means mechanically, constraints
  - `max_length`: Maximum character count (for UI elements with fixed width)
  - `flags`: `FIXED_SIGNAL`, `ALIGNMENT_VARIANT`, `VOICE_VARIANT`, `INTERPOLATED`
  - `screenshot`: Reference to a screenshot showing the string in context (generated during development)

### Unicode and Font Support

| Language Group | Requirements |
|----------------|-------------|
| Latin (EN, ES, PT, FR, DE) | UTF-8, standard Latin Extended character set. Accent marks, special characters (n-tilde, c-cedilla, umlauts). |
| Japanese | UTF-8, full JIS X 0208 coverage. Requires CJK font with kanji, hiragana, katakana. Minimum ~6,000 kanji. |
| Korean | UTF-8, full KS X 1001 coverage. Requires Hangul font with complete jamo combinations (~11,172 syllable blocks). |
| Simplified Chinese | UTF-8, GB 2312 minimum (6,763 characters). GB 18030 preferred for full coverage (~27,000+ characters). |
| Arabic (future) | UTF-8, Arabic Unicode block. RTL text rendering. Contextual glyph shaping (initial, medial, final, isolated forms). |

**Font requirements:**
- Each supported language needs a tested font that covers its character set at all UI sizes (combat log, NPC dialogue, tooltips, headers).
- CJK languages require larger minimum font sizes for readability (12px minimum vs 10px for Latin).
- Font fallback chain: primary font per language -> secondary font -> system default.

### Text Expansion Handling

| Language | Typical Expansion vs English | Impact |
|----------|------------------------------|--------|
| German | +25-35% | UI panels must accommodate longer strings. Combat log lines may wrap. |
| French | +15-25% | Moderate. Most UI fits. Some button labels need review. |
| Spanish | +15-25% | Similar to French. |
| Portuguese | +15-25% | Similar to French. |
| Japanese | -20-30% (fewer characters, but wider per character) | Character count is lower but pixel width may be similar or larger depending on font. |
| Korean | -10-20% | Similar to Japanese. |
| Chinese | -30-40% (fewest characters) | Shortest strings but widest individual characters. |

**Design rules:**
- UI elements use flexible-width containers where possible.
- Fixed-width elements (buttons, headers) are tested with German text (worst-case expansion).
- Combat log uses monospace-compatible fonts with tested line wrapping per language.
- Maximum string length constraints in resource files are set per-language, not globally.

### RTL Language Support (Future)

Arabic support requires:
- Mirrored UI layout (navigation on right, content flows right-to-left)
- Bidirectional text rendering (numbers and English terms within Arabic text remain LTR)
- RTL-aware text input for player names and chat
- Combat log template engine must support RTL string concatenation

**Decision:** RTL support is deferred. It requires UI framework changes that should not block Phase 2 or Phase 3 languages. When Arabic is prioritized, allocate a dedicated engineering sprint for RTL infrastructure.

---

## Localization Workflow

### Per-String Lifecycle

```
Author (EN) -> Context Doc -> Translation -> In-Context Review -> QA -> Deploy
```

| Stage | Owner | Deliverable |
|-------|-------|-------------|
| **1. Authoring** | Content designer | English source string + context comment + flags + max_length |
| **2. Context documentation** | Localization coordinator | Screenshot, mechanical explanation, tone guide reference, related strings |
| **3. Professional translation** | External translation vendor (per language) | Target-language string, translator notes |
| **4. In-context review** | Native-speaker QA (per language) | String reviewed in its actual UI/log context. Tone, length, clarity verified |
| **5. Functional QA** | QA team | Interpolation tested, `FIXED_SIGNAL` consistency verified, text expansion checked, no truncation |
| **6. Deployment** | Engineering | Resource files updated, build deployed, smoke test per language |

### Continuous Localization (Live Service)

Stormbound Online is a live service game. New strings arrive with:
- Monthly balance patches (patch notes, adjusted descriptions)
- Seasonal updates (new content, new NPCs, new quest text)
- Emergency hotfixes (player-facing communication)
- New behavioral voices and combat log variants

**Process:**
- New strings are tagged with a version/patch number.
- Translation pipeline runs continuously — new English strings enter the queue immediately on commit.
- Phase 2/3 languages may lag English by up to 1 week for standard updates.
- Emergency hotfix communications are translated within 24 hours for all supported languages.
- Seasonal content translations are completed and reviewed before the season goes live (no partial translations at season launch).

### Community Translation Feedback

- Each supported language has a community feedback channel (forum thread or Discord channel).
- Players can report translation issues: incorrect tone, mechanical signal mismatch, truncation, typos.
- Reports are triaged by a native-speaker community moderator (volunteer or contracted).
- Verified issues enter the translation pipeline as fixes with priority above new strings.
- Monthly translation quality report published internally: issues reported, issues fixed, languages with highest error rates.

---

## What NOT to Localize

| Category | Reason |
|----------|--------|
| Build codes (e.g., `SB-IRN-RAD-Tk4v9`) | Machine-readable identifiers. Must be universal across all languages for build sharing. |
| Behavioral hash (e.g., `#7CF2A`) | Generated fingerprint. Language-agnostic. |
| Gem domain IDs (internal keys) | Backend identifiers. Never shown to players in raw form. |
| API responses | Developer-facing. Not player-visible. |
| Debug logs | Developer-facing. Not player-visible. |
| Player-generated content (names, chat messages) | Players author in their own language. No translation applied. |
| Currency symbols and number formatting | Handled by locale formatting rules, not string translation. Numbers use locale-appropriate separators (1,000 vs 1.000). |

---

## Localization Budget Drivers

| Factor | Impact on Cost | Mitigation |
|--------|---------------|------------|
| Alignment triple-variant dialogue | 3x NPC dialogue string count | Prioritize story NPCs for full 3-variant treatment. Ambient NPCs can share alignment-neutral lines where tone difference is minimal. |
| Behavioral voice variants | 6x combat log narrative count | MVP ships 3 voices. Remaining voices are lower-priority translation tasks. |
| CJK font licensing | Per-language font license cost | Use open-source CJK fonts (Noto Sans CJK) where quality permits. License commercial fonts only for display/header text. |
| Creative localization (voice writing) | Higher per-word cost than standard translation | Batch voice writing by archetype — one writer handles all Predator strings for their language in a single contract. |
| Ongoing live service strings | Continuous translation cost | Build translation memory (TM) database. Reuse approved translations for repeated patterns. Seasonal content is planned 4 weeks ahead to avoid rush pricing. |

---

## Related Pages

- [[Combat Logs System]] — Narrative signal language, combat log structure
- [[Behavioral Identity]] — Combat log voice variants, archetype system
- [[NPC System]] — Alignment-variant dialogue, NPC categories
- [[Gem Identity System]] — Domain naming, archetype generation
- [[UI Design]] — Panel layout, text display, color language
- [[Meta Management]] — Patch cadence and seasonal cycle
- [[Accessibility System]] — Font size, color contrast (overlaps with localization font requirements)
- [[Patch & Communication System]] — Player-facing communication that must also be localized
