---
tags:
  - gdd
  - ui
  - audio
---

# Audio Design

## Design Philosophy

Stormbound Online is a text-based game. There are no character models, no animated environments, no particle effects. In the absence of visual spectacle, **audio carries the atmosphere**. Sound is the primary sensory channel that separates "reading a combat log" from "being in a fight."

Three principles govern audio design:

1. **Sound as Environment** --- Audio creates the space that text describes. When the location header says "Stormreach," the player should hear it before they read it.
2. **Sound as Feedback** --- Every player action produces an audio response. The reactive UI is not just visual --- it is sonic. A commit lock, a gem socket, a marketplace sale --- each has a distinct sound that becomes associated with its function through repetition.
3. **Sound as Information** --- Audio encodes game state. A player who listens closely should be able to sense momentum shifts, mutation thresholds, and boss phase transitions through sound alone. This parallels the [[Combat Logs System]]'s philosophy: the game rewards attention across all channels.

**Constraint:** Audio must never be required. All information conveyed through sound is also conveyed through text and UI. Audio enhances --- it does not gate. Players who mute the game lose atmosphere, not functionality.

---

## Music

### Regional Themes

Each region has a signature musical identity that reflects its dominant gem domain ecology, narrative tone, and gameplay purpose.

| Region | Musical Identity | Instrumentation | Mood | Notes |
|--------|-----------------|-----------------|------|-------|
| **Port Haven** (Calm Shores) | Warm, welcoming, unhurried | Acoustic guitar, soft strings, light percussion, distant wind instruments | Safe harbor, community, rest | The "home" sound. Players should feel relief when this music plays after a difficult dungeon. Tempo: 70-80 BPM |
| **Calm Shores** (Overworld) | Oceanic ambient, gentle exploration | Ocean pads, harp arpeggios, seagull samples, rolling low-frequency waves | Discovery, openness, the beginning of a journey | Aegis-domain influence: consonant harmonies, resolved chords, stable tonality |
| **Stormreach** (Hub: Thunder Arena) | Tense, competitive, electric | Distorted strings, metallic percussion, thunder samples woven into rhythm | Ambition, conflict, proving oneself | Tempest-domain influence: driving rhythms, staccato patterns, building intensity |
| **Stormreach** (Overworld) | Volcanic intensity, perpetual storm | Low brass, anvil strikes, distant thunder rolls, crackling electrical ambience | Danger, adaptation, power | Kinetics secondary influence: percussive impacts, displacement-suggestive panning |
| **Verdant Depths** (Post-MVP) | Mysterious, organic, layered | Wooden flutes, tuned percussion, bioluminescent synth textures, forest samples | Wonder, hidden knowledge, patience | Synthesis-domain influence: layered melodies that build on each other like constructs |
| **Obsidian Wastes** (Post-MVP) | Oppressive, decaying, hollow | Reversed strings, granular decay textures, subsonic drones, void-like silences | Dread, temptation, the cost of power | Entropy-domain influence: sounds that degrade over time, notes that erode |
| **Celestial Peaks** (Post-MVP) | Ethereal, expansive, resonant | Crystal bowls, choir pads, wind chimes, echo-heavy piano | Achievement, legacy, transcendence | Resonance-domain influence: heavy use of reverb and delay, every note echoes |

### Combat Music

Combat music operates on an **intensity layer system** that responds to game state in real time.

#### Layer Structure

| Layer | Content | Trigger |
|-------|---------|---------|
| **Base** | Rhythmic foundation, sparse instrumentation | Combat begins |
| **Tension** | Harmonic movement, melodic fragments | Player or opponent below 70% Integrity |
| **Drive** | Full instrumentation, increased tempo | Momentum 7+ (Advantage) for either side |
| **Climax** | Peak intensity, aggressive melody | Momentum 9-10 (Dominance) OR either side below 30% Integrity |
| **Resolution** | Deceleration, harmonic resolution | Combat ends (victory or defeat) |

#### Momentum-Music Mapping

| Momentum Zone | Musical Response |
|---------------|-----------------|
| 0-1 (Desperation) | Stripped instrumentation, dissonant undertones, heartbeat-like pulse. The music communicates danger |
| 2-4 (Pressure) | Tense, minor-key phrasing, irregular rhythm. Uncomfortable but not panicked |
| 5-6 (Neutral) | Balanced intensity. Neither winning nor losing feels urgent |
| 7-8 (Advantage) | Driving rhythm enters. Melodic confidence. The player feels momentum in the sound |
| 9-10 (Dominance) | Full orchestration. Triumphant motifs. Speed increase. The music says "finish it" |

#### PvP vs PvE Music

- **PvE combat:** Regional theme influences combat music. Fighting in Stormreach uses Tempest-flavored instrumentation. Fighting in Calm Shores uses Aegis-flavored tones
- **PvP combat:** Neutral combat theme with no regional flavor. Both players hear the same music. Momentum layers respond to the listening player's momentum, not the opponent's. This means two players in the same fight hear different intensity levels

#### Boss Themes

Each expedition dungeon boss has a unique theme that layers over the standard combat music.

| Boss | Region | Musical Identity |
|------|--------|-----------------|
| Tidecaller Caverns Boss | Calm Shores | Oceanic choir, building wave rhythms, tidal crescendos. The music feels like the ocean breathing |
| Storm Leviathan (Stormspire) | Stormreach | Full orchestral storm. Lightning strike accents on boss phase transitions. Relentless, rolling thunder percussion |
| Thornmother (World Boss, Verdant Depths) | Post-MVP | Organic instrumentation that grows more complex as the fight progresses --- mirroring the Synthesis domain's construct-building mechanic |

Boss themes override the regional combat music entirely. Players should immediately recognize "this is a boss fight" from the audio alone.

### Menu and Login Music

| Context | Music |
|---------|-------|
| **Login Screen** | A slow, contemplative arrangement of the Port Haven theme. Establishes the game's identity. 90 seconds before loop. No percussion --- pure melody and pads |
| **Character Select** | Gentle variant of the Calm Shores theme. Warm but anticipatory |
| **Loading Screens** | Region-appropriate ambient music. No percussion, low volume. Functional background, not a performance |

---

## Ambient Soundscapes

Each region has a persistent ambient layer that plays beneath music and responds to context.

### Regional Ambience

| Region | Primary Sounds | Secondary Sounds | Dynamic Elements |
|--------|---------------|-----------------|-----------------|
| **Port Haven** | Harbor water lapping, rope creaking, distant crowd murmur | Tova's workshop (anvil strikes, gem grinding), seagulls, cargo being loaded | Time-of-day variation: busier during "peak hours," quieter at night. Ambient NPC chatter volume scales with player population in the hub |
| **Calm Shores** (Overworld) | Ocean waves (variable intensity), coastal wind, sand shifting | Bird calls, distant ship horns, rustling palms | Wave intensity increases near Tidecaller Caverns. Gem deposits emit a soft hum when the player is nearby |
| **Stormreach** (Thunder Arena) | Crowd murmur (larger and more aggressive than Port Haven), arena clash echoes, forge hammering | Lightning crackle, flag snapping in wind, distant thunder | Crowd volume spikes when a ranked match completes. Arena clash sounds come from random directions, suggesting active fights |
| **Stormreach** (Overworld) | Continuous low thunder, volcanic rumble, sulfur wind | Rock cracking, electrical arcing, distant eruption booms | Thunder frequency increases near Stormspire. Electrical sounds intensify in Tempest-dominant sub-zones |
| **Verdant Depths** (Post-MVP) | Canopy rustle, insect chorus, dripping water | Bioluminescent hum, ancient mechanism clicks, root creaking | Sound density increases deeper in the jungle. Near the Druid Circle, a subsonic pulse (the Driftmind's presence) is felt more than heard |
| **Obsidian Wastes** (Post-MVP) | Ground vibration drone, decay hiss, wind over dead stone | Shadow whisper, void crackle, distant structural collapse | Ambient decay sounds grow louder near Rift Zones. The Corruption Font has a heartbeat-like subsonic pulse |
| **Celestial Peaks** (Post-MVP) | Wind chimes (crystal), high-altitude wind, distant choir pad | Echo reflections of player actions, cloud movement hiss | Resonance hum intensifies near the Radiant Sanctum. Player actions echo more prominently at higher altitudes |

### Dungeon Ambience

| Dungeon Type | Audio Character | Signature Sounds |
|-------------|----------------|-----------------|
| **Farming Dungeons** | Intimate, enclosed | Close-wall echo, dripping, small creature movement, gem deposit hum |
| **Expedition Dungeons** | Expansive, threatening | Deep reverb, distant boss presence (rumble, breath, movement), environmental hazard audio |
| **Mythic Dungeons** | Oppressive, layered | Multiple ambient layers that shift between rooms. Silence used deliberately. Audio stingers at mechanic trigger points |

### Hub Ambience

| Hub | Signature | Notes |
|-----|-----------|-------|
| **Port Haven** | Harbor sounds + marketplace activity + NPC voices | The "comfort" soundscape. Should feel populated and safe |
| **Thunder Arena** | Crowd + combat echoes + weather | The "competition" soundscape. Energetic, slightly aggressive |
| **Sky Forum** (Post-MVP) | Wind chimes + echo + distant craft sounds | The "prestige" soundscape. Serene, elevated, exclusive |

---

## Combat Audio

### Domain Sound Identity

Each gem domain has a distinct sonic signature that plays when its actions are used. This creates an audio "fingerprint" for every build --- a player wielding Tempest/Kinetics sounds fundamentally different from one wielding Synthesis/Resonance.

| Domain | Sound Signature | Action Audio Examples |
|--------|----------------|----------------------|
| **Kinetics** | Deep impacts, displacement whoosh, spatial panning | Force Strike: heavy thud + directional whoosh. Gravity Well: reverse-suction sound. Momentum Burst: escalating impacts |
| **Entropy** | Corrosion hiss, dissolving crackle, material degradation | Corrode: acid sizzle. Decay Chain: spreading crackle. Destabilize: glass stress fracture. Unravel: fabric tearing |
| **Resonance** | Bell tones, harmonic overtones, delayed echoes | Harmonic Strike: clean bell + delayed echo. Amplify: rising pitch swell. Frequency Shift: doppler effect. Dampening Wave: reverb cutoff |
| **Void** | Hollow whoosh, negative-space suction, silence punctuation | Consume: vacuum inhale. Null Zone: complete silence (2-second audio gap). Siphon: draining gurgle. Absence: fade to nothing |
| **Flux** | Morphing timbres, unstable pitch, texture shifting | Shift Strike: sound that changes mid-playback. Metamorphosis: glitch-like audio stutter. Adaptive Shield: frequency matching sweep. Phase Walk: phase-shifted footstep |
| **Tempest** | Electrical crackle, chain lightning, building static | Storm Strike: sharp crack + static build. Lightning Chain: cascading zap. Overclock: electrical overload whine. Surge: massive discharge boom |
| **Aegis** | Shield ring, resonant metal, absorption thud | Fortify: shield materialization clang. Reflect: metallic ricochet. Endure: deep absorption boom. Sanctuary: warm tonal wash |
| **Synthesis** | Construction clicks, crystalline assembly, layered tones | Construct: building-block assembly sequence. Enhance: pitch elevation. Merge: two tones harmonizing into one. Catalyze: cascade detonation |

### Phase Transition Sounds

Each combat phase has a signature audio cue that marks the transition.

| Transition | Sound | Duration | Notes |
|-----------|-------|----------|-------|
| **Round Start** | Low chime + subtle drum hit | 0.5s | Consistent, recognizable. The "new round" sound |
| **Draw Phase** | Card shuffle/fan sound (abstract, not literal cards) | 0.8s | Audio representation of actions being drawn. Plays once per draw |
| **Commit Lock** | Mechanical lock click + brief tone | 0.3s | Satisfying, final. The "no going back" sound. Should feel weighty |
| **Clash Begin** | Impact cymbal + tension rise | 0.5s | Marks the moment both players' commits are revealed |
| **Slot Resolution** | Per-action sounds (see domain table above) | Variable | Each slot resolves with its domain-appropriate audio |
| **Aftermath Begin** | Settling tone, tension release | 0.8s | The "dust settles" sound. Transition from action to information |
| **Round End** | Soft resolution tone | 0.3s | Brief, clean. Prepares the ear for the next round start |

### Momentum Shift Audio

| Momentum Change | Audio Cue |
|----------------|-----------|
| +1 Momentum | Ascending two-note chime (subtle) |
| -1 Momentum | Descending two-note tone (subtle) |
| Entering Advantage (7) | Confidence stinger: brief brass hit + driving rhythm fades in |
| Entering Dominance (9) | Triumph stinger: full chord + combat music shifts to Climax layer |
| Entering Pressure (4) | Warning tone: muted brass, minor key |
| Entering Desperation (1) | Alarm: heartbeat-like pulse begins, dissonant undertone |

### Gem Stability Audio

| Stability Event | Audio Cue | Notes |
|----------------|-----------|-------|
| Stability drop (per use) | Faint crystalline stress sound | Barely audible. Subconscious signal |
| Entering Shifting (75) | Distinct tone shift + subtle warble | The gem's "voice" changes. Attentive players notice |
| Entering Unstable (50) | Audible instability crackle | Clear warning. Cannot be missed if volume is on |
| Entering Critical (25) | Fracture sound + alarm undertone | Urgent. The gem is about to break |
| Gem Breaks (0) | Shattering glass + silence | The silence after the shatter is the most important part. 1.5-second audio gap |
| Mutation Available | Subtle harmonic shift | Post-combat only. A "something changed" tone |
| Overcharge | Electrical overload whine + power surge | Loud, dramatic. Both players hear it |

### Resonance Audio

| Resonance Event | Audio Cue |
|----------------|-----------|
| Proximity hint (3 rounds away) | Faint harmonic hum beneath combat audio |
| Building (2 rounds away) | Harmonic hum increases in volume, begins to pulse |
| Imminent (1 round away) | Clear dual-tone chord that matches the two participating domains |
| **Trigger** | Full resonance stinger: both domain sounds play simultaneously, merge into a unique combined tone, then resolve. 1.5-2 seconds. This should be one of the most satisfying sounds in the game |
| Opponent resonance trigger | Same stinger but slightly muffled/distant. The player knows something happened but the audio communicates "not yours" |

---

## UI Feedback Sounds

Every interactive UI element produces audio feedback. Sounds are short (under 0.5s), distinct, and never annoying on repetition.

### General UI

| Action | Sound | Character |
|--------|-------|-----------|
| Button click | Soft mechanical click | Clean, satisfying, neutral |
| Panel open | Smooth slide + light chime | Expansive, welcoming |
| Panel close | Reverse slide (shorter) | Brief, definitive |
| Tab switch | Page-turn rustle | Light, navigational |
| Scroll | Subtle tick per item passed | Minimal, rhythmic |
| Error / Invalid action | Low buzz + muted thud | Not harsh. Communicates "no" without punishment |
| Success / Confirmation | Bright ascending two-note chime | Positive, brief |
| Toggle on | Click + ascending tone | |
| Toggle off | Click + descending tone | |

### Gem and Build UI

| Action | Sound | Character |
|--------|-------|-----------|
| Gem socket (equip) | Crystalline lock-in + resonant hum | Satisfying, mechanical. The gem "seats" into place |
| Gem unsocket (unequip) | Reverse crystalline release | Quick, clean |
| Build code copy | Clipboard snap + subtle chime | Functional, confirming |
| Build card inspect | Paper unfold + gem hum | Thematic, brief |
| Loadout change | Multi-gem resettling sound | All four gems adjusting simultaneously. Complex but short |
| Mutation lock-in | Deep crystalline shift + new harmonic | The gem's "voice" permanently changes. Should feel weighty |
| Mutation revert | Crystalline snap-back + return to original tone | Relief/safety sound |

### Marketplace and Economy

| Action | Sound | Character |
|--------|-------|-----------|
| Item listed | Soft chime + coin clink | Business-like, satisfying |
| Transaction complete (buy) | Register ding + coin cascade | Classic purchase satisfaction |
| Transaction complete (sell) | Coin collection + brief fanfare | Slightly more rewarding than buying |
| Commission posted | Stamp/seal sound | Official, contractual |
| Commission completed | Seal break + success chime | Fulfillment |
| Auction outbid | Alert tone (minor key) | Not alarming, but attention-getting |

---

## Notification Audio

Notifications use a strict hierarchy of audio urgency. Each category has a unique sound signature that players learn to recognize without looking at the screen.

### Notification Hierarchy

| Priority | Category | Sound | Character | Volume Relative to Master |
|----------|----------|-------|-----------|--------------------------|
| **Critical** | Match found (dungeon/PvP queue) | Three-note ascending chime, bright and urgent | Demands attention. Must cut through ambient audio | 100% |
| **High** | World boss spawning, alignment war event start | Two-note horn call | Important but not time-critical within seconds | 85% |
| **Medium** | Marketplace sale, commission completed, guild event | Pleasant single ding (varies by type) | Positive, informational. Does not demand immediate action | 70% |
| **Low** | Friend request, friend online, social notification | Soft single tone | Gentle, non-intrusive. Ambient awareness | 55% |
| **Ambient** | System message, patch note available, codex update | Subtle click | Barely registers unless the player is attentive. Background information | 40% |

### Notification Rules

- Notifications never interrupt combat audio. They queue and play during Aftermath phase or between rounds
- Duplicate notifications within 5 seconds are batched into a single sound (prevents spam during busy marketplace periods)
- Critical notifications play regardless of other audio. All other categories respect a "notification cooldown" of 2 seconds between sounds
- Players can preview all notification sounds in the Audio Settings panel
- Each notification category can be independently muted

---

## Adaptive Music System

Music is not a playlist. It is a responsive system that reads game state and adjusts in real time.

### Music State Machine

The music system tracks the following variables and blends between layers accordingly:

| Variable | Values | Musical Response |
|----------|--------|-----------------|
| **Location** | Region, sub-zone, hub, dungeon | Selects base theme and instrumentation palette |
| **Combat state** | Out of combat, in combat, boss fight | Transitions between exploration, combat, and boss music |
| **Momentum** | 0-10 scale | Controls combat intensity layer (see Combat Music section) |
| **Gem instability** | Highest instability among equipped gems | Adds dissonant undertones as instability increases. At Critical (25), a persistent warble enters the mix |
| **Alignment** | Radiant / Corrupted / Unbound | Subtle tonal coloring. Radiant: major inflections. Corrupted: minor/diminished inflections. Unbound: modal ambiguity |
| **Boss phase** | Phase 1 / 2 / 3 / Enrage | Boss theme evolves. Each phase adds instrumentation and intensity. Enrage strips everything to percussion and a single driving melody |
| **Time in session** | Continuous | After 30+ minutes in the same region, ambient layers gradually shift to prevent audio fatigue. New melodic fragments surface. Familiarity breeds variation |

### Transition Rules

| Transition | Method | Duration |
|-----------|--------|----------|
| Region to region | Crossfade with 3-second overlap | 3s |
| Exploration to combat | Rhythmic alignment: combat music enters on the next downbeat of the ambient track | 1-2s (synced to tempo) |
| Combat to exploration | Instruments drop out sequentially over 4 seconds. Ambient fades back in | 4s |
| Normal combat to boss | Hard cut on boss encounter trigger. Boss stinger plays (1s), then boss theme begins | 1s stinger + immediate theme |
| Boss phase transition | Musical stinger (unique per boss) + layer addition. No gap in music | 0.5s stinger, seamless layer add |
| Victory | Resolution chord held for 2 seconds, then gentle fade to regional ambient | 2s hold + 3s fade |
| Defeat | Music cuts to silence for 1 second, then a low, subdued variation of the regional theme fades in | 1s silence + 3s fade |

### Adaptive Layers

The music system uses a minimum of 4 simultaneous audio layers:

| Layer | Content | Always Playing |
|-------|---------|---------------|
| **Pad/Atmosphere** | Sustained tones, harmonic foundation | Yes |
| **Rhythm** | Percussion, pulse, tempo carrier | Only in combat or hubs with activity |
| **Melody** | Thematic material, recognizable motifs | Intermittent. Surfaces and retreats |
| **Reactive** | Momentum stingers, stability warbles, resonance hints | Triggered by game state only |

---

## Audio Settings

Players have granular control over their audio experience. No channel is bundled.

### Volume Controls

| Channel | Default | Description |
|---------|---------|-------------|
| **Master** | 80% | Controls all audio output |
| **Music** | 70% | Regional themes, combat music, boss themes, menu music |
| **SFX** | 80% | Combat action sounds, domain audio, phase transition cues |
| **UI** | 60% | Button clicks, panel sounds, gem socket sounds, build card interactions |
| **Combat Feedback** | 80% | Momentum shift cues, stability warnings, resonance audio, overcharge sounds |
| **Ambient** | 50% | Regional soundscapes, environmental audio, hub ambience |
| **Notifications** | 60% | All notification categories (individual muting available per category) |
| **Voice/Chat** | 70% | Reserved for voice chat integration (Post-MVP). Placeholder in MVP settings |

### Additional Audio Options

| Setting | Options | Default | Notes |
|---------|---------|---------|-------|
| **Mono Audio** | On / Off | Off | Collapses stereo field for accessibility. All spatial audio cues converted to volume/timing differences |
| **Reduce Sudden Sounds** | On / Off | Off | Compresses dynamic range. Loud stingers and combat impacts are softened. For players sensitive to sudden volume spikes |
| **Notification Sound Preview** | Per-category playback | N/A | Lets players hear each notification type and adjust volume or mute individually |
| **Combat Audio Priority** | On / Off | On | When enabled, combat audio ducks (reduces volume of) ambient and notification audio during fights |
| **Music During Loading** | On / Off | On | Some players prefer silence during loading screens |
| **Heartbeat at Low HP** | On / Off | On | The Desperation heartbeat pulse can be disabled for players who find it anxiety-inducing |

### Accessibility Considerations

- All audio-conveyed information has a visual equivalent. Momentum shifts show in the log. Stability warnings appear as text. Resonance proximity uses narrative signal language (see [[Combat Logs System#Resonance Signals]])
- Screen reader compatibility: UI sounds are designed to not conflict with screen reader output
- Audio descriptions of combat events are handled by the combat log text, not by audio-only cues

---

## Related Pages

- [[UI Design]] --- Panel layout, color language, reactive interface framework
- [[Combat Logs System]] --- Narrative signal language, information tiers
- [[Combat System]] --- Phase structure, momentum, stability
- [[Gem Identity System]] --- 8 gem domains and their systemic rules
- [[World Design]] --- Regional atmosphere, gem domain ecology
- [[Onboarding System]] --- First hour audio experience, progressive disclosure
- [[GDD Hub]] --- Master document index
