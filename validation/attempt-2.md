# Validation Attempt 2 — Hypothetical Finance Calendar Week

**Provenance: every figure in this document is HYPOTHETICAL.** Stipulated
figures are the fixture's actuals (per `fixture_actuals`). Hand
arithmetic used under the validation hand-math exception; production
runs go through the Calculator API.

## Fixture declaration (per `drafting_and_fixtures.fixture_schema`)

- `as_of_date`: **2026-09-18** (Friday)
- `timezone`: **America/Detroit** (declared fixture setup input, EDT)

**Stipulated transaction ledger (all hypothetical)** — morning balances
reflect postings through the prior day:

- Mon 9/14: Coffee $6.45; Work food $11.30
- Tue 9/15: Groceries $54.20; Fuel $41.00
- Wed 9/16: Fast food $18.75; Dining out $42.10; Coffee $5.80
- Thu 9/17: Roth IRA transfer $130.00 (posts); Work food $9.75
- Fri 9/18: Huntington car payment $497.39 (posts); Paycheck $2,688.39
  (arrives); Fast food $14.25
- Sat 9/19: Groceries $63.40; Coffee $7.20

**Key figures (all hypothetical):** opening current balance $1,900.00;
bills $130.00 Roth IRA (due Thu 9/17), $497.39 Huntington car payment
(due Fri 9/18); paycheck $2,688.39 (arrives Fri 9/18; next Fri 10/02);
month-end horizon 2026-09-30; safety target (starter floor) $2,380.00;
stipulated reserve saved $1,475.00 → $905.00 to go; watch threshold
$250.00 (kit default).

**Safe-to-spend derivation (as_of_date, Friday):** $1,580.65 (current) +
$2,688.39 (in-horizon, unarrived paycheck) − $497.39 (in-horizon,
unposted bill) = $3,771.65. The Roth IRA posted Thursday, so it is
inside the balance and out of the subtraction.

All events use **6:00 AM–12:00 PM America/Detroit** blocks.

## Daily balance events (7)

### 1. Mon 2026-09-14 — `$1,900.00 balance` — graphite (8)

```
$3,961.00 safe to spend
SAFE TILL SEP 30 — $3,961.00 safe
Safety target (starter floor): $2,380.00 — $905.00 to go

Current: $1,900.00
Available: $1,862.55

Upcoming bills:

SEP 17 — $130.00  Roth IRA transfer

SEP 18 — $497.39  Huntington car payment

Next paycheck:

SEP 18 — $2,688.39  Paycheck

Month-to-date spending:

$412.86  Other living costs (6 charges)

$96.20  Fast food (non-work) (4 charges)

$45.60  Work food (4 charges)

$38.00  Fuel (1 charge)

$27.99  Amazon (1 charge)

$0.00  Finance (investment transfers) (0 charges)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

──────────────
$620.65  Total

Watch: none
```

### 2. Tue 2026-09-15 — `$1,882.25 balance` — graphite (8)

```
$3,943.25 safe to spend
SAFE TILL SEP 30 — $3,943.25 safe
Safety target (starter floor): $2,380.00 — $905.00 to go

Current: $1,882.25
Available: $1,850.00

Upcoming bills:

SEP 17 — $130.00  Roth IRA transfer

SEP 18 — $497.39  Huntington car payment

Next paycheck:

SEP 18 — $2,688.39  Paycheck

Month-to-date spending:

$412.86  Other living costs (6 charges)

$102.65  Fast food (non-work) (5 charges)

$56.90  Work food (5 charges)

$38.00  Fuel (1 charge)

$27.99  Amazon (1 charge)

$0.00  Finance (investment transfers) (0 charges)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

──────────────
$638.40  Total

Watch: none
```

### 3. Wed 2026-09-16 — `$1,787.05 balance` — graphite (8)

```
$3,848.05 safe to spend
SAFE TILL SEP 30 — $3,848.05 safe
Safety target (starter floor): $2,380.00 — $905.00 to go

Current: $1,787.05
Available: $1,755.00

Upcoming bills:

SEP 17 — $130.00  Roth IRA transfer

SEP 18 — $497.39  Huntington car payment

Next paycheck:

SEP 18 — $2,688.39  Paycheck

Month-to-date spending:

$467.06  Other living costs (7 charges)

$102.65  Fast food (non-work) (5 charges)

$79.00  Fuel (2 charges)

$56.90  Work food (5 charges)

$27.99  Amazon (1 charge)

$0.00  Finance (investment transfers) (0 charges)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

──────────────
$733.60  Total

Watch: none
```

### 4. Thu 2026-09-17 — `$1,720.40 balance` — graphite (8)

```
$3,781.40 safe to spend
SAFE TILL SEP 30 — $3,781.40 safe
Safety target (starter floor): $2,380.00 — $905.00 to go

Current: $1,720.40
Available: $1,690.40

Upcoming bills:

SEP 17 — $130.00  Roth IRA transfer (due today)

SEP 18 — $497.39  Huntington car payment

Next paycheck:

SEP 18 — $2,688.39  Paycheck

Month-to-date spending:

$467.06  Other living costs (7 charges)

$169.30  Fast food (non-work) (8 charges)

$79.00  Fuel (2 charges)

$56.90  Work food (5 charges)

$27.99  Amazon (1 charge)

$0.00  Finance (investment transfers) (0 charges)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

──────────────
$800.25  Total

Watch: none
```

### 5. Fri 2026-09-18 — `$1,580.65 balance` — FLAMINGO (4) ← as_of_date

```
$3,771.65 safe to spend
SAFE TILL SEP 30 — $3,771.65 safe
Safety target (starter floor): $2,380.00 — $905.00 to go

Current: $1,580.65
Available: $1,548.20

Upcoming bills:

SEP 18 — $497.39  Huntington car payment (due today)

Next paycheck:

SEP 18 — $2,688.39  Paycheck

Month-to-date spending:

$467.06  Other living costs (7 charges)

$169.30  Fast food (non-work) (8 charges)

$130.00  Finance (investment transfers) (1 charge)

$79.00  Fuel (2 charges)

$66.65  Work food (6 charges)

$27.99  Amazon (1 charge)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

──────────────
$940.00  Total

Watch: none
```

(Roth IRA posted Thursday → dropped from upcoming bills, now in MTD
Finance; Huntington due today → flagged; paycheck arrives today →
listed unflagged.)

### 6. Sat 2026-09-19 — `$3,757.40 balance` — graphite (8)

```
$3,757.40 safe to spend
SAFE TILL SEP 30 — $3,757.40 safe
Safety target (starter floor): $2,380.00 — $905.00 to go

Current: $3,757.40
Available: $3,725.00

Upcoming bills:

none

Next paycheck:

OCT 02 — $2,688.39  Paycheck

Month-to-date spending:

$964.45  Other living costs (8 charges)

$183.55  Fast food (non-work) (9 charges)

$130.00  Finance (investment transfers) (1 charge)

$79.00  Fuel (2 charges)

$66.65  Work food (6 charges)

$27.99  Amazon (1 charge)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

──────────────
$1,451.65  Total

Watch: none
```

### 7. Sun 2026-09-20 — `$3,686.80 balance` — graphite (8)

```
$3,686.80 safe to spend
SAFE TILL SEP 30 — $3,686.80 safe
Safety target (starter floor): $2,380.00 — $905.00 to go

Current: $3,686.80
Available: $3,686.80

Upcoming bills:

none

Next paycheck:

OCT 02 — $2,688.39  Paycheck

Month-to-date spending:

$1,027.85  Other living costs (9 charges)

$190.75  Fast food (non-work) (10 charges)

$130.00  Finance (investment transfers) (1 charge)

$79.00  Fuel (2 charges)

$66.65  Work food (6 charges)

$27.99  Amazon (1 charge)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

──────────────
$1,601.24  Total

Watch: none
```

## Weekly food report (1)

**Sun 2026-09-20 — `-$129.60 Food report (pending)` — banana (5)**
(week-to-date Mon–Thu actuals as of Friday morning: Groceries $54.20
(1); Dining out $42.10 (1); Work food $21.05 (2); Coffee $12.25 (2);
total $129.60)

```
$54.20  Groceries (1 charge)

$42.10  Dining out (1 charge)

$21.05  Work food (2 charges)

$12.25  Coffee (2 charges)

──────────────
$129.60  Total

SAFE TILL SEP 30 — $3,771.65 safe
```

## Recurring bill events (2)

**Thu 2026-09-17 — `$130.00 Roth IRA` — peacock (7)**

```
Due: THU SEP 17
Monthly — Roth IRA transfer
Amount: $130.00 (hypothetical)
```

**Fri 2026-09-18 — `$497.39 Huntington` — peacock (7)**

```
Due: FRI SEP 18
Monthly — car payment
Amount: $497.39 (hypothetical)
```

## Paycheck event (1)

**Fri 2026-09-18 — `$2,688.39 Paycheck` — sage (2)**

```
Arrives: FRI SEP 18
Biweekly
Amount: $2,688.39 (hypothetical)
```
