# v0.2 validation report — reserve floor + Shield

**All figures in this report are hypothetical.** Validator read only the seven kit files (`method-kit.json`, `SKILL.md`, `METHOD.md`, `visual-spec.md`, `snapshot-contract.md`, `README.md`, `CHANGES.md`); `method-kit.json` treated as authoritative on conflicts. Arithmetic was done by hand under the kit's hand-arithmetic exception.

Outputs: `validation/attempt-v02-a.md` (Fixture A, 11 events), `validation/attempt-v02-b.md` (Fixture B, 8 events).

## Fixture A — derivation check (hypothetical)

- raw_safe = 1900.00 + 2688.39 − 497.39 − 130.00 = **$3,961.00**
- buffer = max(0, min(3961.00, 2400.00 − 5000.00)) = **$0.00** — reserve $5,000.00 ≥ one month of essentials $2,400.00 → healthy → no buffer. Matches METHOD.md's worked example exactly.
- Shield: 5000.00 ÷ 2380.00 = 2.1008 → **100%** (capped); level **shielded** (reserve ≥ starter target); bar rendered 10 filled — the cap is a validator interpretation; see finding F2.

## Fixture B — derivation check (hypothetical)

- raw_safe = 500.00; buffer = max(0, min(500.00, 2400.00 − 0.00)) = **$500.00**; safe = **$0.00**
- Shield: 0.00 ÷ 2380.00 = 0 → **0%**; level **unshielded**; no-reserve rendering.

## Verdicts

### (a) Fixture B holds back the starter buffer — PASS

Line 1 of every daily balance event in Fixture B reads character-exactly:

`$0.00 safe to spend — $500.00 held back as a starter buffer because no emergency reserve is on record.`

This matches `reserve.floor_rule.no_reserve_statement` word-for-word, amounts at two decimals. The $500.00 checking balance is never advertised as safe: Line 1 says $0.00 safe, and the SAFE TILL line reads `SAFE TILL SEP 30 — $0.00 safe`. The $500.00 figure appears only where it belongs — the `Current:`/`Available:` balance pair — not in any spendable position.

### (b) Fixture A applies no buffer — PASS

Buffer computes to exactly $0.00 (healthy reserve ≥ one month of essentials), so per `reserve.floor_rule.healthy_reserve` Line 1 stays the plain `$3,961.00 safe to spend` with no buffer language. The computation reduces to the v0.1 formula, as the kit promises.

### (c) Shield section renders correctly in both — PASS (with F2 caveat)

- Fixture A: `Shield: 🛡️ shielded — $5,000.00 of $2,380.00 starter target (M1 Finance)` + `▓▓▓▓▓▓▓▓▓▓ 100%`. Icon is 🛡️ (U+1F6E1 U+FE0F), level name `shielded`, target_name `starter`, ` (M1 Finance)` appended after the target name per `line_1_where`. Two lines, no heading, directly after the safety-target line with no blank line.
- Fixture B: `Shield: 🛡️ unshielded — no emergency reserve on record` + `░░░░░░░░░░ 0%` — the exact no-reserve rendering.
- Caveat: the 10-character cap on the over-100% bar is a validator interpretation, not a kit rule — see F2.

### (d) Previously validated behavior — PASS on all checks

- **Flamingo/grey:** exactly one flamingo balance event per fixture — the `as_of_date` (A: MON SEP 21; B: FRI SEP 25); the other six are graphite (8). Non-as_of events carry the full note body per `note_scope`.
- **SAFE TILL format:** `SAFE TILL SEP 30 — $3,961.00 safe` (A) / `SAFE TILL SEP 30 — $0.00 safe` (B) — amount next to the heading, never alone on the final line.
- **Amount-first rows, sorted high-to-low:** MTD sections sort descending; $0 lines last in canonical `category_lines` order (A: Finance, Car maintenance, Other Apple Pay costs; B: Finance, Work food, Amazon, Car maintenance, Other Apple Pay costs).
- **Divider:** exactly 14 U+2500 (`──────────────`), combined total last as `$<amount>  Total` with no charge count. (Blank-line treatment around the divider is a validator choice — see F9.)
- **Watch line:** single line, no separate heading — A: `Watch: unknown merchant — $78.44 (unrecognized merchant, verify)`; B: `Watch: none`.
- **Posted/arrived drop-off:** the fixture stipulates nothing posts or arrives during the week, so both bills and the Sep 28 paycheck stay listed and stay in the safe computation all week. The date-relative flags were exercised: on THU SEP 24 the Roth IRA row carries ` (due today)` and on FRI SEP 25 the car-payment row carries ` (due today)` in those days' notes; the Sep 28 paycheck never enters the week, so no ` (arriving today)` flag appears. The `Next paycheck:` section shows the in-horizon paycheck (A) / renders `none` via the empty-section rule (B).
- **Charge-count grammar:** `(1 charge)` singular, `(0 charges)`/`(n charges)` otherwise, applied throughout. Two decimals on all amounts. Titles lead with the amount (`$1,900.00 balance`, `-$43.55 Food report (pending)`, `$497.39 Huntington car payment`, `$2,688.39 Paycheck`). All events are 6:00 AM–12:00 PM America/Detroit blocks. Colors: 4/8 daily, 5 weekly, 7 bills, 2 paycheck. Bill events sit on due dates; the paycheck event sits on the arrival date.

## Adversarial findings (v0.2 additions)

Severity key: **C** = contradiction (kit says two incompatible things), **A** = ambiguity (two runtimes can both claim conformance and produce different bytes), **I** = insufficiency (a reachable case has no specified rendering).

### F1 — C: The no-reserve statement lies about partial reserves

`reserve.floor_rule.no_reserve_statement` is keyed to **buffer > 0**, but its text asserts *"…because no emergency reserve is on record."* A partial reserve below one month of essentials also produces buffer > 0. Example (hypothetical): reserve $2,399.00 on record, essentials $2,400.00, raw_safe $100.00 → buffer = min(100, 2400 − 2399) = $1.00, safe $99.00. The mandated Line 1 reads "$99.00 safe to spend — $1.00 held back as a starter buffer because no emergency reserve is on record." — factually false: a $2,399.00 reserve IS on record. The golden-fixture scenario (c) ("partial reserve below one month of essentials — reduced buffer of (essentials − reserve)") specifies the math but no Line-1 wording, so a conforming validator must print a false statement. Fix: key the exact statement to the no-record case and specify a partial-reserve Line-1 variant (e.g. "…held back as a starter buffer because the recorded reserve is below one month of essentials").

### F2 — C/A: The fill bar is undefined when shield_fill > 100%

`shield.fill` says the percent is "capped at 100 for display," but `shield.report_section.line_2` defines the bar as `▓` repeated `round(shield_fill × 10)` with no cap. Fixture A: fill = 2.1008 → round(21.008) = **21 segments** — impossible in a "10-character fill bar." visual-spec.md says "round(fill × 10) filled, the rest empty," which is likewise unrenderable for fill > 1. The validator capped the bar at 10 to produce output, but a second runtime could emit 21 segments and still claim conformance. The portability gate requires byte-identical output, so this is release-blocking: the kit must state "filled segments = min(10, round(fill × 10))" (or equivalent) in both files.

### F3 — C: The kit's own Shield example disagrees with itself

METHOD.md: $2,500.00 ÷ $14,400.00 = 17.36% → rendered "17%" with bar `▓▓░░░░░░░░` (2 of 10 = 20%). visual-spec.md repeats the same pair. The kit claims "The number and the picture are generated from the same figures, so they can never disagree" — but the picture implies 20% while the number says 17%, because the percent rounds to the nearest 1% while the bar rounds to the nearest 10%. Same-figures generation does not prevent numeric/visual disagreement under independent rounding granularities. Fix: either derive the bar from the rounded percent (e.g. `round(pct_displayed ÷ 10)` segments) or soften the never-disagree claim.

### F4 — C: visual-spec's flow-chart example doesn't add up

visual-spec.md §3 gives a "character-exact template" with hypothetical example `Flow: $2,688.39 in → $627.39 bills → $100.00 reserve → $3,961.00 safe`. 2688.39 − 627.39 − 100.00 = **$1,961.00**, not $3,961.00. The example's safe figure equals `in − bills` and silently drops the $100.00 reserve-transfer segment, contradicting the method's rule that scheduled non-spend transfers are subtracted. (Mitigating: §3 states the chart is deliberately NOT part of the v0.2 daily note layout — but a pinned "character-exact" template with internally inconsistent example numbers is still a defect.)

### F5 — A: The `(where)` parenthetical has no no-reserve rule

`shield.report_section.line_1_where` covers the case "when the user gave a 'where' for the reserve." `line_1_no_reserve` has no `(where)` slot. Reachable cases with no specified rendering:
- User confirms a reserve exists and gives `where`, but declines the amount → record_fields has no "amount unknown" representation (`amount` is a number, 0 when no reserve on record), and levels define `unshielded` as "no reserve on record (amount 0 or unknown)" while `report_section` keys the no-reserve line off "no reserve on record" — a user who *confirmed* a reserve exists would be rendered as "no emergency reserve on record."
- `where` given, amount declined, exists=true: is the `where` silently dropped from the Shield line, or appended? Unspecified.
- SKILL.md step 2b says "If the user declines to answer, record 'no reserve on record'" — but partial decline (answers where, declines amount) is not "declines to answer" and has no recording rule.

### F6 — A: The `guarded` level is unreachable for starter targets below essentials

Levels: `guarded` = "one month of monthly_essentials ≤ reserve < shield_target_amount." When `shield_target_amount ≤ monthly_essentials` — true in **both fixtures** ($2,380.00 < $2,400.00) — the guarded band is empty: a reserve of $2,390.00 is simultaneously ≥ essentials (healthy, no buffer) and ≥ target (`shielded`), jumping straight from `building` to `shielded`. The four levels read as an exhaustive ladder but one rung can vanish. Same for a job_loss target with N=1. The kit should state whether this is intended (guarded only meaningful when target > essentials).

### F7 — A/I: The floor ignores `shield_target`; the kit never says so on purpose

The reserve floor's healthy threshold is ≥ one month of essentials regardless of `shield_target`. A job_loss-target user with $5,000.00 recorded against a $14,400.00 target gets Line 1 plain (no buffer) while the Shield reads `guarded` at 35%. Whether the buffer should ever respect the chosen job-loss target is a real user-facing question the kit never addresses; CHANGES.md says the floor is deliberately one month of essentials, but no kit file states the floor *intentionally* ignores `shield_target`. Related wording issue: the no-reserve statement always says "starter buffer" even when the user chose a job-loss target — potentially confusing.

### F8 — A: Buffer edge when raw_safe < essentials has no worked rendering

With no reserve and raw_safe below essentials (e.g. raw_safe $100.00 → buffer $100.00, safe $0.00), Line 1 reads "$0.00 safe to spend — $100.00 held back as a starter buffer because no emergency reserve is on record." — conformant, but the kit shows only the $500.00 case. Combined with F1, validators have no example for buffer ∈ (0, raw_safe) or for partial-reserve buffers at all.

### F9 — A: Blank lines around the MTD divider are unspecified

`note_layout.row_spacing` mandates one blank line *between rows*; the divider is not a row, and neither file pins spacing around it or between it and the total row. The validator rendered blank line → divider → blank line → total, but "divider directly under last row, total directly under divider" is equally conformant — a byte-level difference under the portability gate. Same gap in the weekly food note ("combined total after divider").

### F10 — A: Percent-rounding ties and bar/percent sourcing unspecified

"Rounded to the nearest percent, capped at 100" — tie-breaking (half-up vs. banker's) is unspecified, and it's unspecified whether the bar rounds from the raw fill or the capped/rounded display percent (matters exactly at the boundary, e.g. fill = 0.95 → bar round(9.5): 9 or 10 depending on tie rule; pct 95%).

### F11 — I: No "no paycheck expected" state

`next_paycheck_scope` says the section "always shows the next expected paycheck," assuming one exists. Fixture B stipulates no paychecks at all and renders `none` via the generic empty-section rule — but a user with genuinely no expected paycheck (vs. an unknown one) has no distinct representation, and `posted_paychecks_drop_off` leaves the section permanently `none` with no "no further paychecks expected" signal. Minor.

### F12 — I: Golden fixture scenario (c) not instantiated this round

`validation.golden_fixtures` lists three reserve scenarios; this round instantiated (a) healthy reserve and (b) no reserve. The partial-reserve scenario (c) — which is exactly where F1 bites — remains uninstantiated. Recommend a fixture with reserve $1,000.00 / essentials $2,400.00 before release, since it is the case the new Line-1 wording cannot currently render truthfully.

## Minor notes (not findings against the kit)

- The task's generic envelope ("2 bill events, 1 paycheck event") conflicts with Fixture B's explicit "no in-horizon bills, paychecks, or transfers at all." The fixture stipulation governed; Fixture B's envelope is 8 events, not 11.
- Fixture B exercises `next_paycheck_scope`'s empty case rather than its out-of-horizon case (no paycheck exists to show past the horizon).
- Fixture A exercises the ` (due today)` bill flag on THU SEP 24 and FRI SEP 25 in those days' notes (bills stipulated not-yet-posted all week). The paycheck arrives MON SEP 28, outside the week, so no ` (arriving today)` flag appears in the envelope.
