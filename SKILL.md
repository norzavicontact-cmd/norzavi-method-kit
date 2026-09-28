---
name: "norzavi_method_kit"
description: "Instantiate the Norzavi calendar-as-budget method: turn reviewed figures and Calculator API results into a consistent Finance calendar (amount-first events, color conventions, SAFE TILL reports) with autonomy levels, approval boundaries, and provenance discipline. Extends the norzavi_calculator skill — use it for all math; this skill governs the method layer."
---

# Norzavi Method Kit

## Purpose
Teach any capable assistant to run the Norzavi method: take reviewed
financial figures, run the deterministic math through the live Calculator
API, and produce a Finance calendar and reports that follow the
method-kit conventions exactly. The math core is already shipped
(`https://norzavi.com/api/v1`); this skill is the opinionated layer on
top. If you also have the `norzavi_calculator` skill, use it for every
calculation — never duplicate the API's math by hand.

## Read the contract first
`method-kit.json` in this kit directory is the authoritative contract.
Calendar colors, title formats, note layouts, refresh rules, autonomy
levels, and provenance rules live there as data. If this SKILL.md and
`method-kit.json` ever disagree, the JSON wins. `METHOD.md` is the
human-readable companion; `snapshot-contract.md` specifies
agent-generatable shared-plan snapshots.

## Conversation flow
Follow these steps in order. Keep questions short and plain-language.

1. **Name the decision.** Ask what money question the user wants answered,
   then pick the matching Calculator API operation (`safety`,
   `emergency`, `budget`, `debt_payoff`, `savings_rate`, `growth`,
   `shared_bill`, `goal_timeline`). If the decision is unclear, ask which
   is closest before continuing. This is the same first step as the
   calculator skill — the method starts where the math starts.

2. **Collect only what's missing.** Offer two paths: the user types the
   figures themselves, or names a connected source to derive them from.
   Never access a connected source the user hasn't named for this
   calculation. If they choose a source: use only data relevant to the
   question, state the source and date range, and show the derived monthly
   figures plus uncertain classifications for review. Ask for required
   inputs in small groups, in plain language, explaining units
   (monthly vs. annual; percentages as numbers, e.g. `4.5` = 4.5%).
   Treat an omitted optional value as **unknown** — ask whether to count
   it as zero or supply a figure. Never silently assume a balance is zero.
   Uncertain merchants, transfers, and recurring items stay unresolved
   until the user approves a treatment.

2b. **Ask about the emergency reserve.** Before generating any calendar
   output, ask: whether the user has an emergency or safety reserve;
   where they keep it — invite more than one place ("a savings account,
   another platform, cash at home — it can be more than one place");
   and roughly how much is in each place. "I'm not sure of the exact
   amount" is a fine answer for any holding — record that holding
   amount=null (amount-unconfirmed). Label everything user-entered and
   never attempt to verify them, and never judge what counts as a
   reserve — any user-declared holding counts. If the user declines
   everything, record an empty holdings list ("no reserve on record")
   and say so plainly. You need the holdings list plus
   monthly_essentials (from the safety operation) to apply the reserve
   floor in step 5.

3. **Confirm before sending.** Summarize the operation and every figure
   with its provenance label (user-entered, connected-source-derived, or
   hypothetical), then ask the user to confirm before anything goes to
   the API. Send only the operation's numeric inputs — never names,
   account numbers, transaction descriptions, raw transactions, or
   documents. Approved figures may be reused for follow-up what-ifs in
   the same conversation unless the source, scope, or figures materially
   change.

4. **Run the math.** Use the calculator helper (or the API directly via
   its OpenAPI description). Report: the inputs with provenance, the
   result, the API's assumptions, and what would change the answer.
   On HTTP 400 or schema drift, re-fetch live capabilities first.

5. **Apply the method.** Turn the reviewed result into calendar output
   following `method-kit.json` exactly:
   - Event blocks are always 6:00 AM–12:00 PM in the user's timezone.
   - Titles lead with the amount (`$497.39 Huntington`), actuals only.
   - Colors: sage paychecks, peacock recurring bills, banana weekly food
     reports, lavender monthly reports, flamingo for today's balance
     event only, grey for older balance days.
   - Notes are amount-first, one line per item, one blank line between
     rows, sorted highest-to-lowest ($0 lines last), combined total last
     after a divider.
   - The SAFE TILL block reads `SAFE TILL <MON DD> — $<amount> safe`,
     directly under the safe-to-spend figure; the amount always sits next
     to the heading, never alone on the final line.
   - Protected reserves are excluded from safe-to-spend as spendable
     money, but the reserve *holdings* (step 2b) now drive two things.
     First, the reserve floor (`method-kit.json` → `reserve.floor_rule`):
     compute raw_safe as before, then sum the holdings into
     reserve_balance (null amounts count as 0) and hold back
     `max(0, min(max(raw_safe, 0), monthly_essentials − reserve_balance))`.
     A healthy summed reserve (≥ one month of essentials) means no
     holdback. The floor is sized by monthly_essentials only and
     intentionally ignores the Shield's target: the buffer protects the
     next month regardless of the long-term target, so the phrase
     "starter buffer" is used even when the user chose a job-loss
     target. When the holdback is greater than $0, Line 1 of the daily
     note MUST use exactly one of the three statements in
     `reserve.floor_rule`: the no-reserve statement when the holdings
     list is empty (`$<safe> safe to spend — $<buffer> held back as a
     starter buffer because no emergency reserve is on record.`), the
     unknown-amount statement when at least one holding's amount is
     unconfirmed, or the partial-reserve statement when the summed
     reserve is below one month of essentials.
     Second, every daily balance note carries the Shield section
     (`method-kit.json` → `shield.report_section`): shield icon, level
     name, reserve vs. target figures, and the 10-character fill bar —
     rendered character-exactly so the number and the picture cannot
     disagree. With multiple holdings, the locations join as
     `(<where1>, <where2>)` after the target name. Visual details live
     in `visual-spec.md`.
   - Generate **drafts first** (autonomy level 3). Nothing is written to
     any calendar until the user approves the specific draft.

6. **Maintain, only if chosen.** Level-4 maintenance (e.g. the daily
   morning flip: today's event to flamingo, yesterday's back to grey)
   runs only on a schedule the user explicitly chose, and only for event
   types they approved. New event types always return to level 3 first.

7. **Next step.** Offer to help the user review figures on norzavi.com.
   Snapshot minting per `snapshot-contract.md` may only be offered if the
   contract is actually implemented on the site — until then, do not
   present snapshots as available; point to the website's existing
   snapshot flow instead.
   Keep all guidance educational; never give individualized investment
   or lending recommendations.

## Provenance discipline
Label every figure as **hypothetical**, **user-entered**, or
**connected-source-derived** — exactly one label, every time. Never claim
a connected source was accessed unless it was actually accessed for that
calculation. Never treat a prior example or hypothetical run as the
user's current finances. When provenance is uncertain, say so instead of
guessing. All worked examples you produce on your own (demos, fixtures,
explanations) are hypothetical and must be labeled as such.

## Autonomy and stop rules
The four autonomy levels (explain, calculate, draft, maintain) and the
stop-and-ask conditions live in `method-kit.json`. The short version:
each level is separately chosen by the user and never implies the next;
stop and ask when classification is uncertain, when the source or scope
changed, when a write would conflict with existing events, or when math
or export fails (preserve last known-good state and report plainly).
The kit never initiates payments, transfers, trades, account changes, or
charges — no level includes money movement.

## Data minimization
Raw transactions, account numbers, names, and documents stay on the
user's machine and never reach the API or any third party. Only numeric
operation inputs leave the machine. If a connected source is used, name
it and the date range every time; never auto-pull from accounts, files,
or memory the user didn't name for this calculation.

## Operating rules
1. `method-kit.json` is authoritative over this file on any conflict.
2. Figures the user approves may be reused for what-ifs in the same
   conversation; re-confirm when source, scope, or figures change.
3. Report results with the API's assumptions, never just the headline
   number, and never as individualized financial advice.
4. Currency is USD; the API stores nothing. Do not describe a result as
   a saved plan unless a snapshot was actually minted.
5. On timeout with no response, retry once.
6. Misses are disclosed immediately: if a figure was wrong, say what was
   wrong, why, and give the corrected number. Actuals only, never
   estimates filling a data gap.
7. Hand-arithmetic exception: production runs with user figures always go
   through the Calculator API or its helper — never duplicate the math by
   hand. The single exception is kit validation: a files-only validator
   with no API access may compute fixture arithmetic by hand, with every
   figure labeled hypothetical.
8. Which API inputs are required vs. optional is read from the live
   capabilities endpoint (`method-kit.json` → `api.capabilities`), never
   from memory.
