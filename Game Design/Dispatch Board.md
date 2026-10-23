---
tags:
  - gdd
  - systems
  - strategy
status: active
---

# Dispatch Board

The strategy layer. During each day's **Watch**, calls come in from across the islands. You decide who goes, who rests, and what you let slip.

## A Call

| Field | Example |
|---|---|
| Title + situation | *Reef fire at Gull Rock.* A lamp-barge has run aground and the oil is burning |
| Needs | **Force 3** and **Resilience 3** (2-3 traits, each with a threshold) |
| Duration | 3 hours of the 8-hour Watch |
| Risk | Low / Medium / High (sets strain and the failure stakes) |
| Reward | Coin, supplies, faction standing, a story lead |
| Expires | End of today. Ignored calls can come back as consequences |

Calls come from three sources: **routine** (generated from a pool), **faction contracts** (move standing), and **story calls** (hand-written, tied to [[Story Structure]] flags).

## Sending Crew

- Send **1 or 2** crew per call. They're unavailable for the call's duration.
- For each required trait, the team uses its **best** value; with two crew, +1 if *both* meet the threshold.
- Relationship modifiers apply ([[Crew & Bonds#The Crew of the Halyard]]).

## Forecast

Shown before you commit, in plain words, and carried over from the combat prototype's forecast idea.

```
margin  = Σ over required traits (team value − threshold)
success = clamp(55 + 15 × margin + relationship + bond perks, 5, 95)
```

| Forecast text | Success range |
|---|---|
| "This is a sure thing." | 85-95% |
| "Good odds." | 65-84% |
| "It could go either way." | 40-64% |
| "This is a long shot." | 5-39% |

The forecast also names *why*: "Brannoc has the strength, but nobody here can take the heat. Send Marlo with him."

## Outcomes

| Result | What happens |
|---|---|
| **Clean success** | Full reward and a short comic strip of them pulling it off |
| **Success with a complication** | Reward, plus a cost: extra strain, a small injury, or a choice on their return |
| **Failure** | Partial or no reward and a consequence (lost standing, an injury, a flag set) |

**Taking the radio:** once per Watch, you can step into a call in progress. It becomes a short scene with a choice that can turn a failure into a success, at a price (a lie, a risk, a promise).

## Strategic Pressure

- 6 crew, 8 hours, 4-6 calls a day: you can't do everything.
- Sending the best person repeatedly raises their gem strain ([[Crew & Bonds#Gem Strain]]).
- Ignored calls aren't just lost rewards; some come back as story consequences.
- Crew who rest instead of working can be talked to at Dusk, so resting has value too.

Related: [[Voyage Calendar]] · [[Crew & Bonds]] · [[Stormbound GDD]]
