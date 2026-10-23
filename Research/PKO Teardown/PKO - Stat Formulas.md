---
tags:
  - research
  - pko
  - combat
  - math
source: https://piratekings.online/database/simulator (character-simulation.js)
captured: 2026-10-23
---

# PKO — Stat Formulas

Reconstructed from the portal's character simulator. It's a client-side port of the server's attribute math, so treat it as authoritative *for this server*. The notation below is mine.

## Primary stats

STR, AGI, CON, SPR, ACC (DEX), plus LUK. LUK exists in the math but isn't player-allocatable in the guides.

**Point income**

- Stat points: `floor(lv/10)·14 + (lv mod 10) + 33` in total, plus `lv − 59` more once lv ≥ 60. That works out to roughly 1 point per level, a +5 bonus every 10th level, and an extra +1 per level after 60.
  - Lv 60 → 118 total, Lv 80 → 166, Lv 90 → 190 (this total includes the starting stats).
- Skill points: `lv − 9`, plus `floor((lv−60)/5)` once lv ≥ 60. That's 61 at Lv 70 and 87 at Lv 90.

> [!note] The guide-derived estimate in [[PKO - Core Systems]] (~133 stat points by Lv80) disagrees with this formula (166 including starting stats). Trust the formula.

## The modifier stack

Every derived stat goes through the same two-layer pipeline:

```text
final = (base × (1 + item_pct/1000) + item_flat) × (1 + skill_pct/1000) + skill_flat
```

- Percent bonuses are in **per-mille** (1000 = +100%).
- Item and skill layers **multiply**, so a % buff from a skill also scales flat item stats. This is why buff classes matter.
- Primary stats run through the same stack *before* the derived stats are computed. Gear "+STR%" therefore compounds twice.
- An off-hand sword works at 20% effectiveness, plus 8% per Dual Sword Mastery level. That one line in the code is what makes the Crusader class work.

## Derived stats (base, before gear)

`k` values are per-class coefficients (table below).

| Stat | Formula | Shape |
|------|---------|-------|
| Max HP | `3·CON·k1 + k2·floor(3·CON/20)² + lv·k_lv + 40` | **Quadratic** in CON |
| Max SP | `3·SPR·1 + k2·floor(3·SPR/20)² + 3·lv + 5` | Linear (k2 = 0 for every class) |
| Min/Max ATK (physical) | `STR·a1 + ACC·d1 + a2·floor(STR/5)² + d2·floor(ACC/5)²` | Quadratic in STR or ACC; each class uses only one of the two |
| Magic ATK | `SPR·m1 + m2·floor(SPR/5)²` | Quadratic in SPR |
| DEF | `5·CON·c1 + c2·(3·CON/20)²` | Quadratic in CON |
| Hit | `floor(ACC·h) + 2·lv + 5` | Linear |
| Flee | `floor(AGI·f) + 2·lv + 5` | Linear |
| Crit | `11 + floor(3·LUK·cr)` | Linear |
| Drop rate (MF) | `100 + floor(3·LUK·0.39)` | Linear |
| Attack interval | `100000 / min(65 + AGI·s, 300)` | Hyperbolic, **hard cap at 300 speed** (AGI ≈ 214 for most classes) |
| HP regen | `2·MaxHP·r1 + 3·CON/8` | Scales off max HP |
| Move speed | base 480, floor at 30% of base | — |

**Design read:** the squared terms make every point worth more than the last. That's the real reason PKO builds are mono-stat ("pure STR champ", "AGI cru"): spreading points gets punished by the convexity. Hit and flee also scale by `2·lv`, so level gaps matter as much as stats.

## Per-class coefficients

| Class | HP/CON k1 | HP/CON² k2 | HP/lv | ATK stat | ATK/stat | DEF/CON² | Flee/AGI | ASPD/AGI | Magic m1 / m2 |
|-------|----------|-----------|-------|----------|----------|----------|----------|----------|---------------|
| Newbie | 3 | 2 | 15 | STR | 1.5 | 0.1 | 0.6 | 1.1 | 1 / 0.4 |
| Swordsman | 5 | 7 | 25 | STR | 1.5 | 0.2 | 0.6 | 1.1 | 1 / 0.4 |
| Hunter | 3 | 3 | 25 | ACC | 1.7 | 0.1 | 0.7 | 1.2 | 1 / 0.4 |
| Explorer / Herbalist | 5 | 5 | 25 | STR | 1.5 | 0.1 | 0.6 | 1.1 | 1.5 / 0.3 |
| **Champion** | 5 | 7 | **40** | STR | **2.0** | **0.6** | 0.6 | 1.1 | 1 / 0.4 |
| **Crusader** | 5 | 7 | 30 | STR | 1.5 | 0.1 | **0.75** | **1.25** | 1 / 0.4 |
| Sharpshooter | 3.5 | 3.5 | 30 | ACC | **2.0** | 0.15 | 0.6 | 1.1 | 1 / 0.4 |
| Cleric | 5 | 5 | 30 | STR | 1.5 | 0.1 | 0.6 | 1.1 | 2 / 0.35 |
| **Seal Master** | 5 | 5 | 30 | STR | 1.5 | 0.1 | 0.6 | 1.1 | **3 / 0.45** |
| Voyager | 5 | 5 | 30 | STR | 1.5 | 0.1 | 0.6 | 1.1 | 2 / 0.35 |

Class identity comes from **a handful of multipliers** on one shared formula:

- Champion gets tank HP per level, the best DEF curve and the best STR scaling.
- Crusader is the only class with boosted flee and attack speed, which explains the 40% Crusader share on the rank ladder (see [[PKO - Tech & Infrastructure]]).
- Seal Master gets 3× linear magic scaling.

## Notable skill buffs (passive or self)

| Skill | Effect |
|-------|--------|
| Sword Mastery | +4 ATK per level |
| Greatsword Mastery | +7 ATK per level |
| Firegun Mastery | +6 min / +10 max ATK per level |
| Berserk | +(20 + 1.5·lv)% ASPD for 20s |
| Blood Bull | +(10 + 2·lv)% HP **and** DEF |
| Strengthen | +20·lv + 3·CON HP |
| Spiritual Fire (Cleric) | +(10 + lv)% ATK, 180 + 20·lv seconds |
| Tempest Boost | +(5 + lv)% move speed and ASPD |
| Angelic Shield | +3%·lv DEF |
| Tornado Swirl | +(5 + lv) crit |

## Takeaways for [[Gem System Math]]

1. **One formula with per-class coefficients** is cheap to balance and easy to read. Stormbound could do the same with gem "affinity" coefficients in place of classes.
2. **Watch convex stat curves.** They create mono-stat metas. If you want hybrid gem builds, use concave or linear curves with soft caps. See [[Stats System]].
3. **Per-mille integer math** avoids float drift on a server. Steal it.
4. **Multiplicative buff layers** make support classes valuable, but they explode at the top end. Cap the skill-layer % or make it additive within each layer.
5. The ASPD hard cap shows a clean "stat stops mattering here" breakpoint. Players love finding those, so expose them in the UI ([[UI Design]]).

---
Back to [[PKO Teardown]]
