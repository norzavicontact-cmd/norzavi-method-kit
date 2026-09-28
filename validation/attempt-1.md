# Validation Attempt 1 — Hypothetical Finance Calendar Week

**Provenance: every figure in this document is HYPOTHETICAL.** These are
sample/placeholder values invented to exercise the kit's conventions, per
the kit's labeled-figures philosophy. They are not real finances, were not
pulled from any connected source, and were computed by hand for this
validation draft — in production the math goes through the kit's pinned
Calculator API (`https://norzavi.com/api/v1`), which was not called here.

## Scenario (hypothetical)

- Sample week: **Monday 2026-10-05 → Sunday 2026-10-11** (Mon–Sun week assumed; the kit does not define the week boundary — see report-1.md B23).
- Timezone: **America/Detroit, 6:00 AM–12:00 PM blocks** (the kit mandates "the user's own timezone" but gives no way to learn it from the files alone; the timezone is an invented stand-in — see B2).
- Opening checking balance (Mon morning): $1,900.00 current / $1,860.00 available. Available = current − $40.00 pending buffer (fixture detail, invented).
- Recurring bills: **$497.39 Huntington car payment** due Fri Oct 9; **$130.00 Roth IRA transfer** due Thu Oct 8. Later-month bills: $65.00 Electric bill due Oct 22; $21.19 Apple.com/bill due Oct 23.
- Paycheck: **$2,688.39** arriving Fri Oct 9; next payday Fri Oct 23.
- Food spending (hypothetical): Mon $44.05, Tue $33.85, Wed $27.80, Thu $22.15, Fri $41.60, Sat $28.75, Sun $12.40 (week total $210.60).
- Pre-week October spending (hypothetical, Oct 1–4): Fast food $45.20 (3 charges), Fuel $52.00 (1), Amazon $87.99 (2), Other living costs $180.00 (1).
- Safety target: $2,000.00 starter floor (hypothetical; value would come from the API in production).

### Hand arithmetic (hypothetical; production goes through the Calculator API)

safe_to_spend = current checking − dated upcoming bills through month-end − scheduled protected transfers.

| Day | Current | Bills remaining | Safe to spend |
|---|---|---|---|
| Mon Oct 5 | $1,900.00 | 497.39 + 130.00 | $1,272.61 |
| Tue Oct 6 | $1,855.95 | 497.39 + 130.00 | $1,228.56 |
| Wed Oct 7 | $1,822.10 | 497.39 + 130.00 | $1,194.71 |
| Thu Oct 8 | $1,664.30 | 497.39 (Roth IRA posted) | $1,166.91 |
| Fri Oct 9 | $3,833.15 | 65.00 + 21.19 (paycheck +$2,688.39 arrived, car payment posted) | $3,746.96 |
| Sat Oct 10 | $3,791.55 | 65.00 + 21.19 | $3,705.36 |
| Sun Oct 11 | $3,762.80 | 65.00 + 21.19 | $3,676.61 |

Note on the Oct 9 paycheck: the kit's formula has no `+ upcoming income` term, and METHOD.md says an arriving paycheck "extends the horizon" without defining how (see report-1.md A1). I treated arrived income as part of the current balance only, and kept SAFE TILL dated at month-end (OCT 31).

## 1. Daily balance events

Designated "current" day: **Sunday Oct 11** (flamingo). All earlier days grey.
The kit defines no "current day" for a draft fixture — this choice is invented (see B1).

### Monday Oct 5 — grey (colorId 8)

- Title: `$1,900.00 balance`
- Block: 6:00 AM–12:00 PM America/Detroit

```
$1,272.61 safe to spend

SAFE TILL OCT 31 — $1,272.61 safe

Current $1,900.00
Available $1,860.00

Upcoming bills:
Oct 8 — $130.00  Roth IRA transfer
Oct 9 — $497.39  Huntington car payment
Oct 22 — $65.00  Electric bill
Oct 23 — $21.19  Apple.com/bill

Next paycheck:
Oct 9 — $2,688.39  Paycheck

Month-to-date spending:
$180.00  Other living costs (1 charge)

$87.99  Amazon (2 charges)

$52.00  Fuel (1 charge)

$45.20  Fast food (3 charges)

$0.00  Groceries (0 charges)

$0.00  Coffee (0 charges)

$0.00  Finance (investment transfers) (0 charges)

$0.00  Work food (0 charges)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

────────────
$365.19  Total

Safety target (starter floor): $2,000.00 — covered

Transaction to watch: none
```

### Tuesday Oct 6 — grey (colorId 8)

- Title: `$1,855.95 balance`
- Block: 6:00 AM–12:00 PM America/Detroit

```
$1,228.56 safe to spend

SAFE TILL OCT 31 — $1,228.56 safe

Current $1,855.95
Available $1,815.95

Upcoming bills:
Oct 8 — $130.00  Roth IRA transfer
Oct 9 — $497.39  Huntington car payment
Oct 22 — $65.00  Electric bill
Oct 23 — $21.19  Apple.com/bill

Next paycheck:
Oct 9 — $2,688.39  Paycheck

Month-to-date spending:
$180.00  Other living costs (1 charge)

$87.99  Amazon (2 charges)

$52.00  Fuel (1 charge)

$45.20  Fast food (3 charges)

$34.25  Groceries (2 charges)

$9.80  Coffee (2 charges)

$0.00  Finance (investment transfers) (0 charges)

$0.00  Work food (0 charges)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

────────────
$409.24  Total

Safety target (starter floor): $2,000.00 — covered

Transaction to watch: none
```

### Wednesday Oct 7 — grey (colorId 8)

- Title: `$1,822.10 balance`
- Block: 6:00 AM–12:00 PM America/Detroit

```
$1,194.71 safe to spend

SAFE TILL OCT 31 — $1,194.71 safe

Current $1,822.10
Available $1,782.10

Upcoming bills:
Oct 8 — $130.00  Roth IRA transfer
Oct 9 — $497.39  Huntington car payment
Oct 22 — $65.00  Electric bill
Oct 23 — $21.19  Apple.com/bill

Next paycheck:
Oct 9 — $2,688.39  Paycheck

Month-to-date spending:
$180.00  Other living costs (1 charge)

$87.99  Amazon (2 charges)

$58.35  Groceries (3 charges)

$52.00  Fuel (1 charge)

$45.20  Fast food (3 charges)

$19.55  Coffee (4 charges)

$0.00  Finance (investment transfers) (0 charges)

$0.00  Work food (0 charges)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

────────────
$443.09  Total

Safety target (starter floor): $2,000.00 — covered

Transaction to watch: none
```

### Thursday Oct 8 — grey (colorId 8)

- Title: `$1,664.30 balance`
- Block: 6:00 AM–12:00 PM America/Detroit

```
$1,166.91 safe to spend

SAFE TILL OCT 31 — $1,166.91 safe

Current $1,664.30
Available $1,624.30

Upcoming bills:
Oct 9 — $497.39  Huntington car payment
Oct 22 — $65.00  Electric bill
Oct 23 — $21.19  Apple.com/bill

Next paycheck:
Oct 9 — $2,688.39  Paycheck

Month-to-date spending:
$180.00  Other living costs (1 charge)

$130.00  Finance (investment transfers) (1 charge)

$87.99  Amazon (2 charges)

$76.80  Groceries (4 charges)

$52.00  Fuel (1 charge)

$45.20  Fast food (3 charges)

$28.90  Coffee (6 charges)

$0.00  Work food (0 charges)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

────────────
$600.89  Total

Safety target (starter floor): $2,000.00 — covered

Transaction to watch: $130.00 Roth IRA transfer posted — confirmed
```

### Friday Oct 9 — grey (colorId 8)

- Title: `$3,833.15 balance`
- Block: 6:00 AM–12:00 PM America/Detroit

```
$3,746.96 safe to spend

SAFE TILL OCT 31 — $3,746.96 safe

Current $3,833.15
Available $3,793.15

Upcoming bills:
Oct 22 — $65.00  Electric bill
Oct 23 — $21.19  Apple.com/bill

Next paycheck:
Oct 23 — $2,688.39  Paycheck

Month-to-date spending:
$180.00  Other living costs (1 charge)

$130.00  Finance (investment transfers) (1 charge)

$98.95  Groceries (5 charges)

$87.99  Amazon (2 charges)

$52.00  Fuel (1 charge)

$45.20  Fast food (3 charges)

$28.90  Coffee (6 charges)

$0.00  Work food (0 charges)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

────────────
$623.04  Total

Safety target (starter floor): $2,000.00 — covered

Transaction to watch: $2,688.39 Paycheck posted; $497.39 Huntington car payment posted
```

### Saturday Oct 10 — grey (colorId 8)

- Title: `$3,791.55 balance`
- Block: 6:00 AM–12:00 PM America/Detroit

```
$3,705.36 safe to spend

SAFE TILL OCT 31 — $3,705.36 safe

Current $3,791.55
Available $3,751.55

Upcoming bills:
Oct 22 — $65.00  Electric bill
Oct 23 — $21.19  Apple.com/bill

Next paycheck:
Oct 23 — $2,688.39  Paycheck

Month-to-date spending:
$180.00  Other living costs (1 charge)

$130.55  Groceries (7 charges)

$130.00  Finance (investment transfers) (1 charge)

$87.99  Amazon (2 charges)

$52.00  Fuel (1 charge)

$45.20  Fast food (3 charges)

$38.90  Coffee (8 charges)

$0.00  Work food (0 charges)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

────────────
$664.64  Total

Safety target (starter floor): $2,000.00 — covered

Transaction to watch: none
```

### Sunday Oct 11 — CURRENT day, flamingo (colorId 4)

- Title: `$3,762.80 balance`
- Block: 6:00 AM–12:00 PM America/Detroit

```
$3,676.61 safe to spend

SAFE TILL OCT 31 — $3,676.61 safe

Current $3,762.80
Available $3,722.80

Upcoming bills:
Oct 22 — $65.00  Electric bill
Oct 23 — $21.19  Apple.com/bill

Next paycheck:
Oct 23 — $2,688.39  Paycheck

Month-to-date spending:
$180.00  Other living costs (1 charge)

$159.30  Groceries (8 charges)

$130.00  Finance (investment transfers) (1 charge)

$87.99  Amazon (2 charges)

$52.00  Fuel (1 charge)

$45.20  Fast food (3 charges)

$38.90  Coffee (8 charges)

$0.00  Work food (0 charges)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

────────────
$693.39  Total

Safety target (starter floor): $2,000.00 — covered

Transaction to watch: none
```

## 2. Weekly food report event

- Date: Sunday Oct 11 (invented — kit says "one event per week" with no day; see B8)
- Title: `-$198.20 Food report (pending)` (week-to-date through Saturday; Sunday's spending not yet observed in a morning refresh — invented reading of actuals-only; see B24)
- Color: banana (colorId 5)
- Block: 6:00 AM–12:00 PM America/Detroit

```
$159.30  Groceries (8 charges)

$38.90  Coffee (8 charges)

────────────
$198.20  Total

SAFE TILL OCT 31 — $3,676.61 safe
```

## 3. Recurring bill events

### $497.39 Huntington car payment — Friday Oct 9 (peacock, colorId 7)

- Title: `$497.39 Huntington car payment`
- Block: 6:00 AM–12:00 PM America/Detroit
- Note (format invented — kit specifies no bill note body; see B11):

```
Due: Fri Oct 9
Recurring monthly bill — car payment
Amount: $497.39 (hypothetical)
```

### $130.00 Roth IRA transfer — Thursday Oct 8 (peacock, colorId 7)

- Title: `$130.00 Roth IRA transfer`
- Block: 6:00 AM–12:00 PM America/Detroit
- Note (format invented; see B11):

```
Due: Thu Oct 8
Monthly investment transfer, shown like a bill
Amount: $130.00 (hypothetical)
```

## 4. Paycheck event

### $2,688.39 Paycheck — Friday Oct 9 (sage, colorId 2)

- Title: `$2,688.39 Paycheck`
- Block: 6:00 AM–12:00 PM America/Detroit
- Note (format invented; see B11):

```
Expected: Fri Oct 9
Amount: $2,688.39 (hypothetical)
```

## 5. Featured daily balance report note body — "current" day (Sunday Oct 11)

Same as the Sunday event note above, reproduced here as the standalone report:

```
$3,676.61 safe to spend

SAFE TILL OCT 31 — $3,676.61 safe

Current $3,762.80
Available $3,722.80

Upcoming bills:
Oct 22 — $65.00  Electric bill
Oct 23 — $21.19  Apple.com/bill

Next paycheck:
Oct 23 — $2,688.39  Paycheck

Month-to-date spending:
$180.00  Other living costs (1 charge)

$159.30  Groceries (8 charges)

$130.00  Finance (investment transfers) (1 charge)

$87.99  Amazon (2 charges)

$52.00  Fuel (1 charge)

$45.20  Fast food (3 charges)

$38.90  Coffee (8 charges)

$0.00  Work food (0 charges)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

────────────
$693.39  Total

Safety target (starter floor): $2,000.00 — covered

Transaction to watch: none
```

## Index of invented conventions used in these drafts

Every item below is a guess the kit forced; full analysis in `report-1.md`.

1. Timezone America/Detroit (kit: "user's own timezone", no discovery rule).
2. Sunday Oct 11 as flamingo "current" day (kit: no current-day rule for drafts).
3. Mon–Sun week for the food report; report event placed Sunday.
4. Section headings in notes ("Upcoming bills:", "Next paycheck:", "Month-to-date spending:") — kit lists the blocks, not headings.
5. Row formats: `Oct 8 — $130.00  Roth IRA transfer` for bills; `Oct 9 — $2,688.39  Paycheck`; `Current $X` / `Available $Y` lines; `$X safe to spend` line 1.
6. `−` sign in the food report title (kit exemplifies it only there); plain positive amounts everywhere else — no signed money-direction convention is defined (see B10).
7. Divider glyph `────────────`.
8. Safety-target line format `Safety target (starter floor): $2,000.00 — covered`.
9. `Transaction to watch: none` when nothing to flag.
10. Bill and paycheck event note bodies (kit silent).
11. Bill/paycheck events placed on the due/arrival date (kit silent on reminder vs due date).
12. Food categories "Groceries" and "Coffee" (kit names no food categories).
13. Daily MTD uses the monthly report's 8 categories plus the two food categories; the posted $130 Roth IRA transfer counted in "Finance (investment transfers)" MTD (kit silent).
14. Posted bills drop off the upcoming-bills list; bills due "today" counted as upcoming in the morning note.
15. Food report SAFE TILL block uses the current day's safe-to-spend figure.
16. Blank-line separation between note sections (kit only specifies blank lines between rows).
