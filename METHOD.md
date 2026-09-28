# The Norzavi Method

*A calendar-as-budget system for people who check their calendar every day
and want it to answer one question at a glance: **what is safe to spend
through the end of the month?***

All figures below are **hypothetical samples** used to illustrate the
method — not real finances.

## The idea in one paragraph

Your calendar already knows your money's rhythm: when bills hit, when
payday lands. Norzavi turns that rhythm into a Finance calendar where
every event carries its amount in the title, the day's balance event
tells you what's safe, and a protected emergency reserve sits in a
separate bucket that never counts as spendable. No login, no backend,
no app subscription — just events on a calendar you already check.

## The two buckets

- **Checking — the spending sensor.** This is the only account the
  method budgets from. Safe-to-spend is computed here.
- **Protected reserve — the emergency bucket.** Held separately (for
  example, in an M1 Finance account). It is **excluded** from
  safe-to-spend entirely — never counted as spendable. But the method
  does record that it exists, roughly how much is in it, and where it
  lives (you tell it; it never checks). That record powers two things:
  the *reserve floor* below, and the *Shield*. The only other trace of
  the reserve in your calendar is the monthly transfer *into* it, shown
  like a bill.

If the reserve counted as spendable, every "safe" number would be a lie.
That separation is the whole game.

## The reserve question

Before the method trusts a safe-to-spend number, it asks about your
emergency reserve: do you have one, where do you keep it — it can be
more than one place — and roughly how much is in each place? Your
answers are taken at face value — reserves usually live where no
assistant can look, and the method never judges what counts as a
reserve: anything you declare counts.

Then comes the **reserve floor**. If your recorded reserve covers at
least one month of essentials, the safe figure computes as normal. If
not, the method holds back up to one month of essentials as a starter
buffer and tells you exactly that. *Sample, hypothetical:* $500.00 in
checking, nothing else moving before month-end, no reserve on record,
$2,400.00 monthly essentials → the whole $500.00 is held back. The
report reads **$0.00 safe to spend — $500.00 held back as a starter
buffer because no emergency reserve is on record.** It refuses to call
$500 "safe" when there is nothing behind it.

A *partial* reserve gets proportional credit. *Sample, hypothetical:*
$3,961.00 raw safe, $2,400.00 monthly essentials, $1,000.00 reserve
recorded at M1 Finance → hold back `min($3,961.00, $2,400.00 −
$1,000.00)` = **$1,400.00**, so safe to spend is **$2,561.00**. The
report reads **$2,561.00 safe to spend — $1,400.00 held back as a
starter buffer because the recorded reserve ($1,000.00) is below one
month of essentials.** (If the user confirms a reserve exists but won't
say how much, the buffer is sized as if the reserve were zero and the
statement says the amount is unconfirmed — never "no reserve on
record.") The floor always protects one month of essentials, whatever
long-term target the Shield uses.

Reserves rarely live in one place, so the method records a **holdings
list** rather than a single account: each holding names where it is
("M1 Finance", "Ally HYSA", "cash envelope") and roughly how much is in
it, and "I'm not sure of the exact amount" is a fine answer for any of
them. The confirmed amounts are summed once and that sum is used
everywhere — the floor and the Shield. *Sample, hypothetical:*
$3,000.00 confirmed in an Ally HYSA plus $2,000.00 cash whose exact
amount you won't state, $3,600.00 monthly essentials, $4,200.00 raw
safe. The confirmed $3,000.00 counts toward the floor, so the holdback
is `min($4,200.00, $3,600.00 − $3,000.00)` = **$600.00** and safe to
spend is **$3,600.00** — but because part of the reserve is unconfirmed,
the report reads **$3,600.00 safe to spend — $600.00 held back as a
starter buffer because the reserve amount is unconfirmed.** The Shield
does its fill math on the $3,000.00 confirmed. One list, summed once,
used everywhere.

## The Shield

The Shield is the reserve's health bar, and it lives in every daily
report. It compares your recorded reserve against your safety target —
either the starter floor or a multi-month job-loss target, your choice —
and fills accordingly:

- **Unshielded** — no reserve on record (or the amount is unconfirmed).
- **Building** — some reserve, less than one month of essentials.
- **Guarded** — at least one month of essentials, below the full target.
- **Shielded** — at or above the target.

*Sample, hypothetical:* $2,500.00 recorded at M1 Finance against a
$14,400.00 job-loss target → guarded, 17% fill:

`Shield: 🛡️ guarded — $2,500.00 of $14,400.00 job-loss target (M1 Finance)`
`▓▓░░░░░░░░ 17%`

The bar is drawn from the displayed percent (nearest whole percent,
capped at 100; ties round half up), so the number and the picture are
consistent by construction — the same figures, one rendering. The exact
icon, fill bar, and level renderings are pinned in the kit's visual
spec, which the website implements too — both sides draw the same
Shield. (One honest edge: when the chosen target is at or below one
month of essentials, the guarded band is empty — a reserve that reaches
essentials is already at or above the target, jumping straight from
building to shielded. Guarded is meaningful when the target exceeds a
month of essentials, e.g. job-loss targets.)

## The daily glance

Each morning there is one balance event for today, in a standout color.
Its title leads with the amount — `$665.60 balance` — and its notes read
top to bottom:

1. **Safe to spend** — the headline number.
2. **SAFE TILL <date> — $<amount> safe** — the same idea, dated. The
   amount always sits next to the heading.
3. **Current vs. available balance** — what the bank says vs. what you
   can actually touch.
4. **Dated upcoming bills plus the next paycheck** — what's leaving and
   what's arriving, in order.
5. **Month-to-date spending by category** — amount first, one line per
   item, biggest at the top.
6. **The transaction to watch** — anything odd, flagged plainly.

Yesterday's balance event fades to grey. Exactly one standout balance
event exists at any time — today's.

## Weekly and monthly reviews

- **Weekly food report.** One event per week. Category rows are
  amount-first (`$129.85  Fast food (3 charges)`), one blank line between
  rows, sorted highest to lowest, `$0` lines last, combined total last
  after a divider, then the SAFE TILL block. The nearest upcoming
  report shows the actual week-to-date total in its title — actuals only,
  never estimates.
- **Monthly spending report.** Runs on the 1st. Category lines cover
  investment transfers, work food, fast food, fuel, Amazon, car
  maintenance, other living costs, and any extra Apple Pay costs — each
  with its total and number of charges.

## Reading an event

- **Titles always lead with the amount:** `$497.39 Huntington`,
  `$130.00 Roth IRA`, `$2,688.39 Paycheck` — two decimals, always.
- **Colors mean things:** paychecks are sage, recurring bills are peacock,
  weekly food reports are banana, monthly reports are lavender, today's
  balance stands out in flamingo, older balance days are grey.
- **Every event is a 6:00 AM–12:00 PM block** — sized so it's easy to
  tap on a phone.

## A hypothetical week

*Sample figures, labeled hypothetical.* Imagine a checking balance of
$1,900.00 on Monday morning, bills of $497.39 (car payment) due Friday and $130.00
(Roth IRA transfer) due Thursday, a $2,688.39 paycheck arriving next
Monday — all before month-end — $2,400.00 monthly essentials, and a
$5,000.00 reserve recorded at M1 Finance.

- **Safe to spend** = $1,900.00 + $2,688.39 − $497.39 − $130.00 =
  **$3,961.00** through month-end, with no floor holdback — the $5,000.00
  reserve is healthy (≥ one month of essentials), so the buffer is $0.
  The horizon stays fixed at month-end: an arriving paycheck doesn't
  extend the date, it increases the amount, because it's money you'll
  have *within* the horizon. (The $130.00 Roth IRA transfer is a
  scheduled non-spend transfer — like the reserve transfer, it's
  committed money, so it's subtracted here; it also shows up once in
  month-to-date spending under Finance. That dual appearance is
  intentional: one is a forward commitment, the other is observed
  spend.)
- The Shield reads guarded-to-shielded against the starter target:
  `Shield: 🛡️ shielded — $5,000.00 of $2,380.00 starter target
  (M1 Finance)` with a full bar — `▓▓▓▓▓▓▓▓▓▓ 100%`.
- Monday's balance event is flamingo: `$1,900.00 balance`, notes show
  `$3,961.00 safe to spend`, `SAFE TILL SEP 30 — $3,961.00 safe`, the two
  dated bills, the dated paycheck, and spending so far.
- Tuesday morning: a new flamingo event for Tuesday; Monday's flips to
  grey. The numbers refresh from actuals — if the car payment posted
  early, it drops off the upcoming list and the safe figure adjusts. No
  estimates fill the gap.

## What the method never does

It never moves money, opens accounts, changes investments, or charges
you. It never stores your transactions, account numbers, or documents —
those stay on your machine. It shows actuals only: if data is stale, it
says so instead of guessing.

## The agent version

If you use an AI assistant with the Norzavi Method Kit, the assistant
runs this same method: it asks what decision you're exploring, confirms
every figure with you before calculating, labels each figure's origin
(you typed it, it came from a source you named, or it's a sample), and
drafts calendar events for your approval before writing anything. You
choose how autonomous it gets — explanations, calculations, drafts, or
scheduled refreshes — and each level is chosen separately.
