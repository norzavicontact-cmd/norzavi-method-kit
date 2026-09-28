# Norzavi Visual Spec — v0.2

The single source of truth for every Norzavi visual. AI/chat renderings
and web renderings both derive from this file: if the assistant's report
and the website ever disagree about what something looks like, this file
decides. On data conflicts (figures, formulas, levels), `method-kit.json`
wins; on visual conflicts, this file wins.

All worked figures below are hypothetical samples.

## 1. Pinned glyphs

These codepoints are pinned. Do not substitute lookalikes.

| Glyph | Codepoints | Use |
|---|---|---|
| 🛡️ | U+1F6E1 U+FE0F | Shield icon (always with VS16) |
| ▓ | U+2593 | Fill-bar filled segment |
| ░ | U+2591 | Fill-bar empty segment |
| ─ | U+2500 | Divider (14×, per method-kit.json) |
| → | U+2192 | Flow-chart arrow (chat rendering) |

Rule: no new glyphs inside validated report elements without a kit
version bump. Pure web-side styling that does not change chat bytes
does not require a bump.

## 2. The Shield

### Chat/text rendering
Exactly per `method-kit.json` → `shield.report_section`: two lines, no
separate heading.

```
Shield: 🛡️ guarded — $2,500.00 of $14,400.00 job-loss target (M1 Finance)
▓▓░░░░░░░░ 17%
```

Line 1: `Shield: <icon> <level> — $<reserve> of $<target>
<target_name> target`, plus ` (<w1>, <w2>)` joining the where-values of
all holdings that have one, in declaration order — e.g. `(M1 Finance,
cash envelope)`. The parenthetical appears only when there is a reserve
to cite: never on the no-reserve line. No-reserve:
`Shield: <icon> unshielded — no emergency reserve on record`.
Amount-unconfirmed (at least one holding's amount declined):
`Shield: <icon> unshielded — reserve amount unconfirmed`, plus
` (<where>)` joining the where-values of the null-amount holdings only —
e.g. `(cash)`; omitted if none of those have a where.
Line 2: filled segments = `min(10, round_half_up(displayed_pct ÷ 10))`,
then `░` to 10 characters, then a space and `<displayed_pct>%`.
Displayed percent = `min(100, round_half_up(fill × 100))`; tie-breaking
is round half up throughout. The bar is derived from the displayed
percent, so the number and the picture are consistent by construction.
No-reserve and amount-unconfirmed: `░░░░░░░░░░ 0%`.

### Web rendering
An inline-SVG shield widget bound to the same `shield_fill` figure:

- viewBox `0 0 24 24`. Outline path (fill none, stroke currentColor):
  `M12 2 L20 6 V12 C20 17 16.5 20.5 12 22 C7.5 20.5 4 17 4 12 V6 Z`
- Fill: the same path used as a clipPath; inside it, a rect anchored at
  the bottom with `height = fill × 24` user units, filled with the
  level color. A track rect in `#E8EAED` sits behind it.
- Level colors (proposed defaults; the level→color mapping must stay
  1:1 and any change is recorded here): unshielded `#9AA0A6`, building
  `#FBBC04`, guarded `#34A853`, shielded `#1A73E8`.
- The widget MUST display the same level name and the same
  reserve/target figures as the chat rendering for the same inputs.
  Same data in, same picture out.
- This is the Shield the website renders at the bottom of the page:
  it fills with reserve ÷ target, not with anything else.

### Animation
- Web: the fill rect animates on data change via
  `transition: height 600ms ease-out`. On a level change, the shield
  pulses once (keyframes `scale(1) → scale(1.06) → scale(1)`, 400ms).
- Chat/text has no animation. The static fill bar IS the visual.
- Parity rule: animation must never carry information the static
  rendering lacks — it may only emphasize what the numbers already say.

## 3. Money-flow chart

Canonical node order, always: **income → bills → reserve → safe-to-spend**.

### Chat/text rendering (character-exact template)

```
Flow: $<in> in → $<bills> bills → $<reserve_transfer> reserve → $<safe> safe
```

Amounts unsigned with two decimals; direction is clear from the labels
(kit sign convention). `<in>` is in-horizon paychecks not yet arrived
plus any other in-horizon income the user confirmed; `<bills>` is
in-horizon bills not yet posted; `<reserve_transfer>` is the in-horizon
scheduled transfer into the protected reserve (`$0.00` when none).
*Hypothetical example (same figures as METHOD.md's worked week):*
`Flow: $2,688.39 in → $627.39 bills → $0.00 reserve → $3,961.00 safe`
Caption rule: the safe node shows the full safe-to-spend figure, which
also reflects the starting balance and the reserve floor — so the chart
is always accompanied by the starting balance it was computed from
(e.g. "from $1,900.00 checking").

### Web rendering
A horizontal 4-node stepper: four cards (label + amount, same labels and
amounts as the chat template, same left-to-right order) joined by →
arrows. Wraps to a vertical stack on narrow screens. Implement with
flexbox; no SVG required.

### Placement
The flow chart is a defined asset for the website and for AI
explanations. It is NOT part of the v0.2 daily note layout — kept out
deliberately to preserve the validated layout. Adding it to a report
layout is a future kit version's decision.

## 4. Icon set

The complete set of icons any kit output may use:

- 🛡️ Shield — reserve health only. Never reused for another meaning.
- ▓ / ░ — Shield fill bar only.
- ─ (×14) — section divider only.
- → — flow-chart direction only.

No emoji in amount-first rows, titles, or section headings. Color
carries meaning on the calendar (method-kit.json colors); icons do not
duplicate it.

## 5. Drift rule

1. Any visual change updates this file first; AI and web re-derive.
2. `method-kit.json` wins on data conflicts; this file wins on visual
   conflicts.
3. A visual change that alters validated report bytes requires a kit
   version bump. Web-only styling that leaves chat bytes untouched does
   not.
