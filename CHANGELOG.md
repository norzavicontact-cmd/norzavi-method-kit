# Method Kit Changelog

## v0.2.1 — reserve holdings list (2026-09-28)

The v0.2 reserve model assumed one reserve in one place. Real users
split savings across locations (a HYSA here, cash there, a separate
platform like M1). The reserve record is now a **holdings list**: each
holding names where it is (free text) and roughly how much is in it,
and "I'm not sure of the exact amount" is a fine answer per holding
(recorded amount=null). The agent never judges what counts as a
reserve — any user-declared holding counts. No account-type taxonomy,
no verification.

### Changes, file by file

- **method-kit.json** — `reserve` section rewritten around
  `holdings[]`: setup questions invite multiple places; record fields
  are per-holding `{where, amount, liquidity_note?}`; derived
  `reserve_balance` (nulls count as 0), `amount_unconfirmed` (any null),
  `confirmed_balance`; floor and Shield math use the summed balance;
  the three Line-1 statements keep their exact wording, with selection
  now keyed off the holdings list (empty → no-reserve; any null →
  amount-unconfirmed; 0 < balance < essentials → partial-reserve);
  Shield `line_1_where` joins multiple locations as `(w1, w2)`, and on
  the amount-unconfirmed line cites only the null-amount holdings'
  locations. Two genuine bugs fixed along the way: the daily-note
  layout rule hardcoded the no-reserve Line-1 wording for *all*
  buffer>0 cases, contradicting the three-statement selection rule
  (now defers to `statement_selection`); the fixture schema still
  described the old `{exists, where, amount}` record (now
  `holdings[]`). Glossary: `reserve_on_record` → `reserve_holdings`.
- **SKILL.md** — step 2b rewritten for the holdings list (invite more
  than one place; per-holding "not sure" accepted; never judge what
  counts); step 5 floor/Shield paragraph updated to sum holdings.
- **METHOD.md** — the reserve question now describes the holdings
  list; new labeled-hypothetical split-reserve example ($3,000.00
  confirmed in an Ally HYSA + $2,000.00 cash amount-unconfirmed,
  $3,600.00 essentials → $600.00 holdback, amount-unconfirmed Line-1
  statement, Shield math on the $3,000.00 confirmed).
- **visual-spec.md** — rendering unchanged (a single summed number
  suffices); only the `(where)` join rule text updated for multiple
  holdings.
- **method-kit.json** — `kit_version` bumped to 0.2.1.

### Validation (v0.2.1)

A fresh validator instantiated two split-reserve fixtures from the kit
files alone. Fixture A ($3,000.00 Ally HYSA confirmed + $2,000.00 cash
amount-unconfirmed, $3,600.00 essentials): Line 1 is exactly the
amount-unconfirmed statement
("$3,600.00 safe to spend — $600.00 held back as a starter buffer
because the reserve amount is unconfirmed."), Shield math uses the
$3,000.00 confirmed sum, Shield line reads
"Shield: 🛡️ unshielded — reserve amount unconfirmed (cash)" with a 0%
bar. Fixture B (both holdings confirmed, $5,000.00 total): plain
"$4,200.00 safe to spend", Shield shielded at 100% with locations
joined as "(Ally HYSA, cash)". All assertions PASS.

Findings fixed: `where` made explicitly optional in
`reserve.record_fields` (the Shield's where-join rule already assumed
it could be absent). Considered and left unchanged: a $3,000 confirmed
+ $2,000 unconfirmed reserve still renders Shield unshielded/0% (the
level rule is explicit — conservative by design); buffer = $0 with no
reserve on record yields a plain "$0.00 safe to spend" Line 1 with no
statement (a $0 figure is not misleading, so no statement is owed).

All validation figures are hypothetical.

## v0.2.0 — reserve-aware safe-to-spend + Shield (2026-09-28, in validation)

v0.1.0 stays release-ready as shipped. v0.2 is behavior-changing: the
safe-to-spend formula gains a reserve floor, the daily report gains a
Shield section, and a shared visual spec is introduced. All figures in
validation artifacts are hypothetical.

### What changed and why

The v0.1 formula answered the wrong question: it computed liquidity
("won't overdraft before month-end"), not safety. $500.00 in checking
with no backup was advertised as $500.00 safe to spend. v0.2 asks about
the emergency reserve at setup and holds back a starter buffer when no
healthy reserve is on record.

### Changes, file by file

- **method-kit.json** — new `reserve` section (setup questions, record
  fields, healthy threshold, floor rule); new `shield` section (target
  selection, fill, four named levels, character-exact two-line report
  rendering); `buckets.computation` updated to
  `safe_to_spend = max(raw_safe, 0) − reserve_floor_buffer`; Line 1 of
  the daily note carries the explicit no-reserve statement when the
  buffer > 0; Shield section placed after the safety-target line;
  glossary updated (`safe_to_spend`, `protected_reserve`) and extended
  (`monthly_essentials`, `reserve_floor_buffer`, `shield`); the
  reserve's recorded amount now has two permitted appearances
  (safety-target line, Shield section) — progress/health, never
  spendable; fixture schema gains `reserve` and `shield_target` fields;
  golden fixtures gain three reserve scenarios; `kit_version` 0.2.0.
- **SKILL.md** — new step 2b (ask the three reserve questions; label
  user-entered; never verify; "no reserve on record" when declined);
  step 5 now applies the floor rule and renders the Shield section.
- **METHOD.md** — the reserve question, the floor rule, and the Shield
  explained with labeled-hypothetical examples, including the $500
  no-reserve case (`$0.00 safe to spend — $500.00 held back as a starter
  buffer because no emergency reserve is on record`).
- **visual-spec.md** (new) — pinned glyphs (🛡️ U+1F6E1 U+FE0F, ▓ U+2593,
  ░ U+2591, ─ U+2500, → U+2192); Shield chat + web (inline SVG) +
  animation renderings with level colors; money-flow chart
  (income → bills → reserve → safe-to-spend) with character-exact chat
  template and web stepper guidance — defined as an asset, NOT added to
  the v0.2 daily note layout; icon set with single-meaning rule;
  animation parity rule; drift rule (visual changes update this file
  first; JSON wins on data, this file wins on visuals).
- **README.md** — v0.2 title; file list updated.

### Validation (2026-09-28)

One fresh validator agent got only the seven kit files (method-kit.json
authoritative) and instantiated two fixture weeks by hand arithmetic,
all figures hypothetical. Outputs:
`validation/attempt-v02-a.md` (healthy $5,000.00 M1 reserve, 11 events),
`validation/attempt-v02-b.md` (no reserve, 8 events),
`validation/report-v02.md` (verdicts + 12 adversarial findings).

- (a) No-reserve week: all 7 days read `$0.00 safe to spend — $500.00
  held back as a starter buffer because no emergency reserve is on
  record.` The $500.00 balance never appears as safe. **PASS.**
- (b) Healthy-reserve week: raw_safe $3,961.00, buffer $0.00, Line 1
  plain — matches METHOD.md's worked example. **PASS.**
- (c) Shield: healthy week renders shielded at 100% with a full bar;
  no-reserve week renders the exact unshielded pair. **PASS.**
- (d) All legacy conventions conform (flamingo/grey, SAFE TILL format,
  amount-first sorted rows, 14-─ divider, charge-count grammar,
  posted/arrived drop-off). **PASS.**

The validator filed 12 findings (F1–F12); all are fixed in this release:

- F1/F5/F8: the no-reserve statement was keyed to buffer > 0 but false
  for partial reserves, and partial decline (exists confirmed, amount
  declined) had no recording rule. Now three Line-1 statements
  (`no_reserve_statement`, `partial_reserve_statement`,
  `unknown_amount_statement`) with an explicit `statement_selection`
  rule; `record_fields.amount` accepts null for amount-unconfirmed;
  `setup_rule` records partial decline as given.
- F2/F3/F10: the fill bar was uncapped above 100% and rounded on a
  different granularity than the percent. Now: displayed percent =
  `min(100, round_half_up(fill × 100))`, bar segments =
  `min(10, round_half_up(displayed_pct ÷ 10))` — the bar derives from
  the displayed percent, so number and picture agree by construction.
  Pinned in both method-kit.json and visual-spec.md.
- F4: visual-spec's flow-chart example didn't add up ($100.00 reserve
  segment silently dropped). Corrected to
  `Flow: $2,688.39 in → $627.39 bills → $0.00 reserve → $3,961.00 safe`
  with a caption rule stating the starting balance alongside the chart.
- F6: documented that the guarded band is empty by construction when
  target ≤ essentials.
- F7: `floor_ignores_target` now states the floor intentionally ignores
  shield_target; "starter buffer" wording is used even for job-loss
  targets.
- F9: divider spacing pinned (divider directly under last row, total
  directly under divider, blank line after) — matches the v0.1 golden
  outputs.
- F11: "none expected" rendering when the user confirms no paycheck is
  expected.
- F12: golden-fixture scenario (c) — partial reserve $1,000.00 vs
  $2,400.00 essentials — hand-verified against the fixed spec:
  buffer $1,400.00, safe $2,561.00, truthful partial-reserve Line 1,
  Shield `building` at 42% (`▓▓▓▓░░░░░░ 42%`).

Validator judgment calls confirmed: (1) capping the over-100% bar at
10 characters — now a pinned rule; (2) Fixture B's 8-event envelope —
the fixture's explicit stipulation governed. The child's fixture
outputs remain conformant under the fixed spec (healthy week's bar and
percent unchanged), so no re-run was needed.

**Verdict: v0.2.0 release-ready.**

## v0.1.0 — validation round 1 (2026-09-28)

A fresh agent with only the kit files instantiated a sample Finance
calendar week (`validation/attempt-1.md`) and filed an adversarial
ambiguity report (`validation/report-1.md`): 6 contradictions, 28
ambiguities. All figures in the validation artifacts are hypothetical.

### Contradictions fixed

- **Paycheck vs. horizon (A1):** the safe-to-spend formula, the worked
  example, and the glossary disagreed on in-horizon paychecks. Resolved
  to one rule: horizon is fixed at month-end; paychecks arriving on or
  before month-end are *added* to the safe amount.
  `safe_to_spend = current balance + in-horizon paychecks − in-horizon
  bills − scheduled non-spend transfers`.
- **Hand-math vs. validation (A2):** the no-hand-math rule now carries
  its explicit exception — files-only validation fixtures may use hand
  arithmetic with hypothetical figures; production always uses the API.
- **Snapshot minting offered but unimplemented (A3):** SKILL.md now
  guards the offer — only if the contract is implemented.
- **Daily flip: standing rule vs. level-4-gated (A4):** refresh rules are
  now labeled LEVEL-4-GATED.
- **Two-decimals vs. `$130 Roth IRA` (A5):** all amounts two decimals.
- **Color-name drift (A6):** banana/peacock used consistently.

### Ambiguities resolved (selected)

- Fixtures/drafts must declare `as_of_date`; that day is flamingo, all
  others grey (B1). Timezone is a required setup input, never assumed (B2).
- Dated bill/paycheck row formats, section headings, divider glyph (14
  U+2500), balance-pair format, Line-1 format, safety-target line format
  and semantics, watch-line selection criteria and format (B3–B7, B13, B21–B22).
- Weekly food report: Monday–Sunday weeks, Sunday placement,
  pending → finalized transition, finalized and placeholder titles,
  4-category taxonomy with monthly roll-up mapping (B8–B9, B14–B16, B23–B24).
- Bill events sit on due dates; posted bills drop off the upcoming list;
  due-today bills stay flagged until observed posted (B11–B12, B17–B18).
- Sign conventions documented; fixture figures declared as the fixture's
  actuals (B10, B19).
- Portability gate made measurable: identical event set and
  character-identical note bodies (excluding run timestamps); fixture
  schema defined (B20).
- "Scheduled protected transfers" renamed in meaning to scheduled
  non-spend transfers (reserve + investment); dual appearance in the
  formula and in month-to-date reporting documented as intentional (B25).
- README validation pointer fixed (B26); API field optionality read from
  live capabilities (B27); stale-data rule: no fabricated flamingo event
  (B28).

## v0.1.0 — validation round 2 (2026-09-28)

A second fresh agent instantiated a hypothetical week (as_of_date
2026-09-18, America/Detroit) from the fixed kit and filed 20 new
findings. Conformance on the 34 round-one fixes: **34/34 clean** — all
six contradictions verified actually fixed (the paycheck/horizon formula
recomputes exactly: $1,900.00 + $2,688.39 − $497.39 − $130.00 =
$3,961.00), and all 28 ambiguities followed. The findings below were
residual/new gaps, fixed here. All validation figures hypothetical.

### Bugs fixed

- **Arrived-paycheck double-count (R2-1):** the round-one formula added
  in-horizon paychecks without excluding already-arrived ones — an
  arrived paycheck sits inside the current balance AND matches
  "arriving on or before month-end," so it was counted twice (a
  Saturday-after-payday safe would have read $6,445.79 instead of
  $3,757.40). Added `posted_paychecks_drop_off`, symmetric to bills:
  once observed arrived, a paycheck drops off the "Next paycheck:" list
  and is never added again. The formula and glossary now read
  "in-horizon paychecks not yet arrived."
- **Watch threshold vs. known bills (R2-16):** the $250.00 default
  threshold literally flagged the known $497.39 car payment every month.
  The threshold now applies to non-recurring charges only; known
  recurring bills and transfers are never watch-flagged for amount.
- **Watch heading/line redundancy (R2-5):** `section_headings` listed
  "Watch:" while the watch_line format already begins with "Watch:" —
  the heading was removed; the watch section is the single line.

### Ambiguities resolved

- **Transfer horizon scope (R2-2):** scheduled non-spend transfers are
  in-horizon only (due on or before month-end), like bills.
- **Weekly SAFE TILL figure (R2-3):** the weekly food report's SAFE TILL
  block carries the refresh day's safe-to-spend figure.
- **Empty sections (R2-4):** render the heading, one blank line, then
  the single line `none`.
- **Total row format (R2-6):** `$<amount>  Total`, no charge count.
- **Charge-count grammar (R2-7):** `(1 charge)` singular;
  `(0 charges)` / `(n charges)` otherwise — pinned for the
  character-identical portability gate.
- **Posted-bill category mapping (R2-8):** posted bills matching no
  named category roll into "Other living costs" (e.g. a car payment).
- **$0-line tie-break (R2-9):** $0 lines keep the canonical
  category_lines order.
- **Top-block and heading spacing (R2-10):** Line 1 → SAFE TILL →
  safety-target line with no blank lines between them; one blank line
  before the balance pair; one blank line between a section heading and
  its first row.
- **Out-of-horizon next paycheck (R2-11):** "Next paycheck:" always
  shows the next expected paycheck; only in-horizon not-yet-arrived
  paychecks enter the safe-to-spend addition.
- **Paycheck arriving today (R2-12):** symmetric ` (arriving today)`
  flag until observed arrived.
- **Safety-target "saved" source (R2-13):** the safety operation's
  `saved` input (same provenance); stipulated in fixtures. Referencing
  it is the one permitted exception to "reserve balance never appears" —
  progress toward the target only, never spendable money.
- **Weekly-note safety-target (R2-14):** omitted from the weekly food
  report; it lives only in the daily balance note.
- **Daily balance title (R2-15):** the title uses the current balance
  (`$1,580.65 balance`).
- **Grey-event note bodies (R2-17):** every daily balance event carries
  the full daily_balance_note structure, including non-as_of days.
- **MTD divider + total (R2-18):** the daily note's month-to-date
  section ends with the divider and combined total row, explicitly.
- **Fixture timezone (R2-19):** in userless fixtures the timezone is a
  stipulated parameter per fixture_schema — declaring it satisfies the
  no-silent-assumption rule.
- **"Observed posted" (R2-20):** the charge appears in actuals from the
  named source, or the user confirms it; stipulated in fixtures.

## v0.1.0 — validation round 3: determinism check (2026-09-28)

Two independent agents received byte-identical kit contents and fixture
parameters (complete text of all six kit files; as_of_date 2026-09-18,
America/Detroit; fixed hypothetical week 2026-09-14→2026-09-20; exact
11-event output envelope). Both produced artifacts saved as
`validation/determinism-a.md` and `validation/determinism-b.md`.
Full evidence in `validation/determinism-report.md`. All figures
hypothetical.

**Result: PASS.** Both outputs byte-identical (md5
`2966a04e222d89c76f7cf2ff8b427450`, 7,577 bytes each, `cmp` clean).
The coordinator independently recomputed every derived figure —
balances, safe-to-spend, MTD totals, food week-to-date, watch selection,
drop-off behavior — and all matched both outputs. No kit changes were
needed; no ambiguities surfaced.

The release gate is satisfied: the Method Kit is **release-ready** at
v0.1.0.
