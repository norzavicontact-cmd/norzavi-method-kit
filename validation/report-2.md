# Validation Report 2 — Adversarial Findings Against the Fixed Kit

**Scope:** a fresh agent with only the six fixed kit files instantiated
the hypothetical week in `attempt-2.md` (as_of_date 2026-09-18,
America/Detroit) and reported ambiguities. All figures hypothetical.

**Round-one conformance check:** the agent verified all six round-one
contradictions as actually fixed (the paycheck/horizon formula recomputes
exactly: $1,900.00 + $2,688.39 − $497.39 − $130.00 = $3,961.00; the
hand-math exception appears in README, JSON, and SKILL.md;
snapshot-offer gating is consistent; the flip is LEVEL-4-GATED;
two-decimal amounts and banana/peacock naming are consistent) and
followed all 28 round-one ambiguities. **Conformance: 34/34.**

## New findings (all fixed in the kit; see CHANGES.md round-2 section)

**R2-1. Safe-to-spend formula double-counts an already-arrived paycheck.**
Read literally, a paycheck that arrived yesterday is inside "current
checking balance" AND "arriving on or before month-end," so it is added
twice (Saturday's safe would read $6,445.79 instead of $3,757.40).
Fixed with `posted_paychecks_drop_off`, symmetric to bills: once
observed arrived, a paycheck drops off the "Next paycheck:" list and is
never added again. Formula and glossary now read "in-horizon paychecks
not yet arrived."

**R2-2. "Scheduled non-spend transfers" unscoped in the computation
string.** Paychecks and bills carried "on or before month-end"
qualifiers; transfers did not. Fixed: transfers are in-horizon only.

**R2-3. Weekly food report's SAFE TILL figure undefined.** "Current day"
could mean refresh day or placement day. Fixed: the refresh day's
safe-to-spend figure.

**R2-4. Empty "Upcoming bills:" section had no rendering.** Fixed:
heading, one blank line, then the single line `none`.

**R2-5. Watch heading vs. watch line redundant.** `section_headings`
listed "Watch:" while the watch_line format already begins with
"Watch:". Fixed: heading removed; the watch section is the single line.

**R2-6. Combined-total row format undefined.** Fixed: `$<amount>  Total`,
no charge count.

**R2-7. Singular vs. plural charge counts undefined.** Fixed:
`(1 charge)` singular; `(0 charges)` / `(n charges)` otherwise.

**R2-8. Non-food posted bills had no monthly-category mapping.**
`posted_bills_drop_off` requires every posted bill to appear in MTD
spending, but the 8 categories had no slot for car payments. Fixed:
posted bills matching no named category roll into "Other living costs."

**R2-9. $0-line tie-breaking undefined.** Fixed: $0 lines keep the
canonical category_lines order.

**R2-10. Heading→first-row and top-block spacing unpinned.** The
portability gate demands character-identical notes, impossible without
pinning these. Fixed: Line 1 → SAFE TILL → safety-target with no blank
lines; one blank line before the balance pair; one blank line between a
section heading and its first row.

**R2-11. "Next paycheck:" scope for out-of-horizon paychecks undefined.**
Fixed: the section always shows the next expected paycheck; only
in-horizon not-yet-arrived paychecks enter the formula.

**R2-12. No "(due today)"-style flag for paychecks arriving today.**
Fixed: symmetric ` (arriving today)` flag until observed arrived.

**R2-13. Safety-target "saved" figure's source undefined.** Fixed: the
safety operation's `saved` input (same provenance); stipulated in
fixtures. Referencing it is the one permitted exception to "reserve
balance never appears" — progress toward the target only, never
spendable money.

**R2-14. Safety-target line in the weekly food report undefined.**
Fixed: omitted from the weekly note.

**R2-15. Daily balance title: current vs. available unspecified.**
Fixed: the title uses the current balance (`$1,580.65 balance`).

**R2-16. Watch threshold literally flags known recurring bills.** The
$250.00 default threshold would flag the $497.39 car payment monthly.
Fixed: the threshold applies to non-recurring charges only.

**R2-17. Grey (non-current) daily balance events' note bodies unstated.**
Fixed: every daily balance event carries the full daily_balance_note,
including non-as_of days.

**R2-18. MTD divider + total in the daily note implied but not explicit.**
Fixed: the daily note's MTD section ends with the divider and combined
total row, explicitly.

**R2-19. Timezone for a userless fixture had no sanctioned non-UTC path.**
Fixed: in userless fixtures the timezone is a stipulated parameter per
fixture_schema — declaring it satisfies the no-silent-assumption rule.

**R2-20. "Observed posted" never operationalized.** Fixed: the charge
appears in actuals from the named source, or the user confirms it;
stipulated in fixtures.

## Method note

Per the task, all arithmetic in `attempt-2.md` was done by hand under
the kit's validation exception, every figure labeled hypothetical. In
production every number is replaced by a Calculator API result.
