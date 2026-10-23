---
tags:
  - gdd
  - systems
  - items
  - ui
status: active
---

# Inventory & Equipment

How players hold, equip, switch and upgrade gear. Item tiers and legendaries are in [[Gear, Relics & Reliquaries]]. A working version exists in the day-one prototype.

## The Hold

- **One shared ship's hold** for everything not being worn: gear and loose gems.
- Each crew member's **worn gear** is separate. You can take gear off one person and give it to another, and they react ([[Character Dynamics]]).
- Later: hold capacity is limited by the ship's size, and a full hold forces choices about what to sell, gift or throw overboard.

## The Character View (paper-doll)

Each crew member is shown as a full figure in the noir style, glowing faintly in the colour of their own gem, with six slots around them:

| Slot | Holds | Mostly affects |
|---|---|---|
| **Head** | Hats, hoods, goggles | Traits on calls |
| **Garment** | Coats and clothes | Traits, and how factions react to them |
| **Tool** | Sextants, axes, lockpicks, stills, hammers | Their profession ([[Professions & Advancement]]) |
| **Gem-housing** | Collars and bracers around their own gem | Gem strain ([[Crew & Bonds#Gem Strain]]) |
| **Keepsake** | Whistles, letters, locks of hair | The psyche ([[Psyche & Trauma]]) |
| **Charm** | Small luck worn at the belt | Small trait bonuses |

- **Equipped gear appears on the figure:** the hat on the head, the coat over the body, the tool in hand, the collar at the neck. Socketed gems show as small coloured stones on the gear.
- Below the figure, the six traits show their total value, with gear bonuses highlighted in ochre.
- Tap a slot to see what's there. Tap an item in the hold to equip it. An empty slot lists everything that fits.

## Gems and Sockets

Loose gems are fragments of the Lattice ([[Lore & Narrative#The Origin of Gems]]). Each domain boosts one trait:

| Gem domain | Trait |
|---|---|
| Kinetics | Force |
| Tempest | Precision |
| Aegis | Resilience |
| Void | Acuity |
| Synthesis | Attunement |
| Entropy | Instinct |

- **Rough shard:** +1. **Cut gem:** +2.
- **Sockets:** Fine gear usually has 1, Masterwork 1-2, Mythic 2, and Legendaries a single "heart" socket with special rules.
- **Resonance:** a gem that matches the wearer's own gem domain gives **+1 more**.
- **Dissonance:** a gem from the opposing domain fights the wearer's gem and adds **+5 strain** to every job they take. The pairs are Kinetics/Void, Tempest/Aegis and Entropy/Synthesis.
- **Removing gems is free and safe.** Nothing ever breaks: a [[PKO Teardown]] avoid-list rule.

## Upgrading

| Upgrade | How | Who |
|---|---|---|
| **Cut a gem** | Two rough shards of the same domain become one cut gem | Nana (or any Smith) |
| **Reforge gear** | Common → Fine → Masterwork, with materials and time | Nana or Brannoc ([[Workshop & Crafts]]) |
| **Awaken a relic** | Story moments only. Legendaries are never upgraded with materials | [[Gear, Relics & Reliquaries#Legendary Relics]] |

Upgrades never fail destructively. A failed reforge wastes materials, never the item.

## How It Feeds Everything Else

- **Dispatch:** traits including gear decide the call forecast, so re-gearing before a Watch is strategy ([[Dispatch Board]]).
- **Strain:** housings reduce it, and dissonant gems add to it.
- **Story:** garments are politics (a Luminari coat in Hollow Court waters), keepsakes trigger memories, and relics open choices.
- **Relationships:** taking someone's gear, or gifting it, moves trust and affection.

## UI Rules

- Opens from a **Crew & gear** button that's always visible, and from each crew card on the Dispatch Board. It's locked only while a choice is on screen.
- Every effect is written in words ("+1 Precision", "fights Marlo's gem"). There are no unexplained icons ([[Motion Comic Presentation]]).
- Thumb-friendly: crew tabs, large slots, and the hold as a two-column list.

Related: [[Gear, Relics & Reliquaries]] · [[Workshop & Crafts]] · [[Dispatch Board]] · [[Stormbound GDD]]
