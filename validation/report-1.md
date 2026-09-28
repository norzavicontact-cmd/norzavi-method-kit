# Validation Report 1 — Ambiguity Findings

**Scope:** a fresh agent instantiated the Norzavi Finance calendar system
using only the five kit files (`README.md`, `method-kit.json`, `SKILL.md`,
`METHOD.md`, `snapshot-contract.md`). No web, no API calls; all arithmetic
done by hand per the task (production use goes through the kit's pinned
Calculator API). Every figure in `attempt-1.md` is labeled hypothetical.

## What was produced

`validation/attempt-1.md` contains a complete hypothetical week
(Mon 2026-10-05 → Sun 2026-10-11): 7 daily balance events with full note
bodies (6:00 AM–12:00 PM blocks; Sunday flamingo as the designated
"current" day, Mon–Sat grey), 1 weekly food report event (banana,
`-$198.20 Food report (pending)`), 2 recurring bill events (peacock), 1
paycheck event (sage), and a standalone featured daily-balance report
note body for the "current" day. All titles lead with the amount; rows are
amount-first, blank-line-separated, sorted high-to-low with $0 lines last
and the combined total after a divider; every SAFE TILL block reads
`SAFE TILL OCT 31 — $<amount> safe` directly under the safe-to-spend figure.

## A. Contradictions and internal tensions

**A1. A paycheck arriving before month-end vs. the fixed month-end horizon vs. a formula with no income term.** Three kit statements pull in different directions:
- `method-kit.json`, `buckets.computation`: `"safe_to_spend = current checking balance − dated upcoming bills through month-end − scheduled protected transfers."` — no income term.
- `METHOD.md`, worked example: `"Safe to spend = $1,900 − $497.39 − $130 = $1,272.61 through month-end (the paycheck arrives before month-end, so it extends the horizon — the method always dates this explicitly in the SAFE TILL block)."`
- `method-kit.json`, `glossary.month_end_horizon`: `"Budgeting runs to the end of the current calendar month, not paycheck-to-paycheck."`

Two readings are possible: (a) the paycheck is *added* to safe-to-spend (but then the JSON formula is wrong); (b) the paycheck merely "extends the horizon," but the glossary fixes the horizon at month-end, so there is nothing left to extend — the SAFE TILL date is OCT 31 either way. I chose a third, literalist reading: ignore unarrived income entirely (formula as written) and date SAFE TILL at month-end; arrived income flows in via the current balance. A portability gate ("same fixture produces materially the same calendar math in every runtime") cannot pass while three statements describe the income treatment differently.

**A2. The kit forbids the very hand-arithmetic the validation task requires.**
- `SKILL.md`: `"If you also have the norzavi_calculator skill, use it for every calculation — never duplicate the API's math by hand."`
- `README.md` validation section: `"a fresh agent with only these files instantiates a sample Finance calendar week"`.

A fresh agent with only these files has no API access in an offline validation run, so instantiation is impossible without violating the "never duplicate the API's math by hand" rule. I resolved it by doing the arithmetic by hand and labeling every result hypothetical (as the task ordered), but the kit as written makes its own validation procedure a rule violation.

**A3. SKILL.md offers snapshot minting that the snapshot contract says not to present as available.**
- `SKILL.md` step 7: `"Offer to help the user review figures on norzavi.com, or to mint a shared-plan snapshot per snapshot-contract.md."`
- `snapshot-contract.md`: `"Do not present snapshots as available until norzavi.com implements this contract"` (status: PROPOSED — not implemented).

An agent following SKILL.md step 7 verbatim will offer an action the contract forbids presenting. The skill needs a guard ("only if the site has implemented the contract").

**A4. The daily flip is both an unconditional standing rule and a level-4-gated action.**
- `method-kit.json`, `refresh_rules.daily_flip`: `"Each morning: create today's balance event in flamingo (4), and flip yesterday's event from flamingo (4) back to grey (8). Exactly one flamingo balance event exists at any time."` — phrased as what happens, no gating.
- `method-kit.json`, `autonomy.levels.4`: maintenance `"may"` only `"refresh previously approved events on the user's explicitly chosen schedule"`, and `"Maintenance covers only event types the user approved. New event types always return to level 3 first."`

Reading 1: the flip happens every morning by default. Reading 2: it happens only if the user explicitly chose level 4 with a schedule naming balance events. The autonomy note says "Each level is separately chosen by the user; choosing a level never grants the levels above it," which supports reading 2 — but then `refresh_rules` should be labeled as level-4 behavior, not as unconditional rules.

**A5. Two-decimals rule vs. the `$130 Roth IRA` example.**
- `method-kit.json`, `title_rules.general`: `"Lead with the amount. Pattern: $<amount> <label>. Two decimals, e.g. $497.39."`
- `METHOD.md`: `"Titles always lead with the amount: $497.39 Huntington, $130 Roth IRA, $2,688.39 Paycheck."`

`$130 Roth IRA` has no decimals. Minor, but a validator checking "two decimals" against METHOD.md's own examples finds a mismatch. (The JSON wins per the stated precedence, so `$130.00` is correct — I used `$130.00`.)

**A6. Color-name drift between METHOD.md and the JSON.**
- `METHOD.md`: `"recurring bills are light blue (peacock)"`, `"weekly food reports are yellow"`.
- `method-kit.json`: `"5": {"name": "banana", ...}`, `"7": {"name": "peacock", ...}`.

"Yellow" vs "banana" is a naming drift; harmless for colorIds but a human reader cross-checking names finds a mismatch.

## B. Ambiguities and silences (each forced a guess)

**B1. No "current day" exists in a draft or fixture.** The kit says `"Only the current day's event is flamingo; older days are grey"` (`title_rules.daily_balance`) and `"Exactly one flamingo balance event exists at any time"` (`daily_flip`). In a drafted 7-day fixture there is no live "today," so flamingo assignment is undefined — a second runtime could equally choose Monday, or none. I designated Sunday Oct 11 (the last day) as flamingo. This alone can break the portability gate.

**B2. "Use the user's own timezone" with no way to learn it.** `method-kit.json`, `calendar.timezone`: `"Use the user's own timezone for all event times. Never convert silently."` From the files alone there is no user, hence no timezone and no default. I assumed America/Detroit as a stand-in. The kit needs either a default or an explicit "ask the user" step.

**B3. No row format for dated bill and paycheck lines in the daily note.** The layout says `"Then: dated upcoming bills plus the next paycheck"` but `row_format` only covers spending rows (`'$129.85  Fast food (3 charges)'`). I invented `Oct 8 — $130.00  Roth IRA transfer` and `Oct 9 — $2,688.39  Paycheck`. Date format, separator, and whether the paycheck carries a `+` sign are all unspecified.

**B4. No format for "current vs. available balance."** I invented two lines, `Current $3,762.80` / `Available $3,722.80`. One line or two, labels, order — all guesses.

**B5. No format for Line 1 (safe-to-spend).** `"Line 1: safe-to-spend figure"` — bare number (`$3,676.61`) or labeled (`$3,676.61 safe to spend`)? I chose labeled. Equally defensible either way.

**B6. The safety-target line has no format and no semantics.** `"Optional: safety-target line (e.g. a starter-floor target) in the month-end summary"`. I invented `Safety target (starter floor): $2,000.00 — covered`. The kit doesn't say whether to show the target, the remaining gap, or a covered/not-covered verdict, nor where "the month-end summary" begins.

**B7. "The transaction to watch" is under-specified.** `METHOD.md`: `"The transaction to watch — anything odd, flagged plainly."` No selection criteria (unrecognized merchant? large amount? duplicate?) and no format for the nothing-to-flag case. I invented `Transaction to watch: none`.

**B8. The weekly food report has no day-of-week.** `"cadence": "weekly"`, `"One event per week."` Sunday? Monday? I placed it on Sunday Oct 11 (end of the sample week).

**B9. Finalized food-report title and placeholder titles are unspecified.** Only the pending form is exemplified: `"-$11.40 Food report (pending)"`. The finalized title (no `(pending)`? sign kept or dropped?) and the "plain placeholders with NO amounts" for further-out weeks (title text?) are both undefined.

**B10. Sign conventions are only partially exemplified.** Spending titles show a leading minus (`-$11.40 Food report (pending)`); every other amount-first title shows a positive (`$497.39 Huntington`). Whether income lines take a `+` (I used `Oct 9 — $2,688.39  Paycheck`, no plus in the end — actually I dropped the `+` in the final drafts and kept plain amounts) and whether negative signs appear inside note bodies is unspecified. A consistent signed-money-direction convention (like the site's negative=out rule) is absent from the kit.

**B11. Bill and paycheck event note bodies are entirely unspecified.** Title, color, and block are specified; the note body is not. I invented minimal bodies (`Due: Fri Oct 9 / Recurring monthly bill — car payment / Amount: $497.39 (hypothetical)`).

**B12. Bill/paycheck event date: due date or reminder date?** I placed events on the due/arrival date. The kit never says whether a bill event is a due-date marker or an advance reminder.

**B13. The divider glyph is unspecified.** `"combined total last after a divider"` — I used `────────────`. Any run of dashes, `---`, or `===` is equally compliant.

**B14. Which amount goes in the food report's SAFE TILL block.** `report_types.weekly_food.contents` lists `"SAFE TILL block"` after the total, but not which figure it carries. I reused the current day's safe-to-spend ($3,676.61). It could equally be the week's own implied figure.

**B15. Food categories are never named.** The monthly report enumerates 8 categories (none of them groceries); the weekly food report says "category rows" with no taxonomy. I invented `Groceries` and `Coffee`. Worse, there is no mapping rule: in the daily MTD I filed groceries under invented `Groceries`/`Coffee` lines alongside the monthly 8, but a strict reading of the kit would force groceries into `Other living costs` — the kit is silent.

**B16. The daily note's MTD category set is unspecified.** `"month-to-date spending by category"` — the same 8 as the monthly report? A subset? I used the 8 monthly categories plus my two invented food categories.

**B17. Posted bills dropping off the upcoming list is implied, never stated.** `"dated upcoming bills"` implies it, and `refresh_rules.actuals_only` supports it, but no rule says "remove a bill from the upcoming list once it posts." I dropped the Roth IRA transfer from Thu onward and the car payment from Fri onward.

**B18. A bill due "today" is neither upcoming nor posted.** In Friday morning's note I listed the Oct 9 car payment under upcoming bills even though it posts that day. The kit gives no intraday rule.

**B19. Hypothetical fixtures vs. "actuals only."** `title_rules.never`: `"Never put an estimate, rolling average, or forecast in a title. Actuals only."` The kit never states that a fixture's stipulated figures count as its "actuals," so a strict reader could argue hypothetical drafts may not use amount-first titles at all. (The portability gate implies fixtures are legitimate inputs, but it doesn't say so outright.)

**B20. The validation gate has no metric and fixtures have no schema.** `validation.golden_fixtures`: `"Keep versioned hypothetical fixtures covering: refunds, transfers between own accounts, pending charges, month-boundary dates, reruns of the same fixture, and full calendar replacement."` — no fixture format is defined. `validation.portability_gate`: `"The same approved hypothetical fixture must produce materially the same calendar math in every runtime"` — "materially the same" is undefined (exact cents? same event set? same colors?). An unmeasurable gate.

**B21. Note section headings are unspecified.** The layout lists blocks ("dated upcoming bills plus the next paycheck," "month-to-date spending by category") but no headings. I invented `Upcoming bills:`, `Next paycheck:`, `Month-to-date spending:`.

**B22. Spacing between note sections is unspecified.** `row_spacing` covers rows ("One blank line between rows"); nothing governs blank lines between sections. I used one blank line between sections, none within the current/available pair.

**B23. The food-report week boundary is undefined.** I assumed Monday–Sunday because the task said so; the kit itself never defines the week (Sunday–Saturday is equally possible and would shift every pending total).

**B24. Pending → finalized transition is undefined.** `pending_refresh`: `"Refresh pending report titles and notes each morning with actuals only."` Nothing says when `(pending)` comes off or what triggers finalization. My Sunday food report is still `(pending)` with week-to-date-through-Saturday figures — a guess at the actuals-only reading for a morning refresh.

**B25. "Scheduled protected transfers" vs. the Roth IRA example.** The JSON formula subtracts `"scheduled protected transfers"`; the glossary defines the protected reserve as emergency savings only, with `"Only the monthly transfer into it appears in the system, shown like a bill"`; but `METHOD.md`'s worked example subtracts a **$130 Roth IRA transfer** — a retirement transfer, not an emergency-reserve transfer. So either the formula's "protected transfers" secretly means "all scheduled investment transfers" (terminology drift), or the METHOD.md example contradicts the glossary. I followed the example (subtracted it) and also counted it in MTD `Finance (investment transfers)` — the double role (bill-like in the safe formula, spending-like in MTD) is itself unspecified.

**B26. README points to a validation file that doesn't exist.** `"See validation/report.md."` The `validation/` directory did not exist before this task, and this report is `report-1.md` per the task — so the README's filename is already drifted from reality.

**B27. "Optional" API inputs are unknowable from the kit.** `SKILL.md` step 2: `"Treat an omitted optional value as unknown — ask whether to count it as zero or supply a figure."` Which inputs are optional lives in the API's OpenAPI description, which is not in the kit — a files-only agent cannot apply this rule.

**B28. Balance-event color on stale-data days is unspecified.** `daily_flip` assumes a fresh event every morning. If source data is stale and no new event can be built with actuals, the kit doesn't say whether the flamingo event stays, goes grey, or is omitted (beyond "say so plainly" in `actuals_only`).

## C. Things the kit required that could not be determined from the files alone

1. The user's timezone (B2) — assumed America/Detroit.
2. Which day is "current" in a draft/fixture (B1) — assumed the last day.
3. The safety-target value (the kit says the figure comes from the `safety` API operation; offline I invented $2,000.00 hypothetical).
4. The food category taxonomy and week boundary (B8, B15, B23).
5. Any note-body content for bill and paycheck events (B11).
6. Whether the sample paycheck should alter the safe-to-spend math or only the horizon date (A1) — the kit's three statements disagree, so no files-only reading is fully consistent.
7. What "materially the same" means for the portability gate (B20) — unmeasurable as written.

## Method note

Per the task, all arithmetic in `attempt-1.md` was done by hand and every
figure is labeled hypothetical. The kit's own rule (`SKILL.md`: "never
duplicate the API's math by hand") conflicts with its own validation
procedure (`README.md`: "a fresh agent with only these files instantiates
a sample Finance calendar week") — see A2. In production, every number
here would be replaced by a Calculator API result with the API's stated
assumptions.
