# Fixture A — healthy off-platform reserve (v0.2 validation)

**All figures in this file are hypothetical.** They are the fixture's stipulated actuals (per `drafting_and_fixtures.fixture_actuals`); every figure carries provenance `hypothetical`. Hand arithmetic was used under the kit's hand-arithmetic exception (`validation.hand_arithmetic_exception`); production runs go through the Calculator API.

## Fixture metadata

- `as_of_date`: 2026-09-21 (the fixture's "today"; flamingo)
- `timezone`: America/Detroit (stipulated fixture parameter)
- Week: MON 2026-09-21 → SUN 2026-09-27
- `shield_target`: starter

## Fixture figures (all hypothetical)

| key | amount | provenance |
|---|---|---|
| checking_current | $1,900.00 | hypothetical |
| checking_available | $1,860.00 | hypothetical |
| bill_huntington_car | $497.39 | hypothetical |
| bill_roth_ira_transfer | $130.00 | hypothetical |
| paycheck_next | $2,688.39 | hypothetical |
| monthly_essentials | $2,400.00 | hypothetical |
| safety_target_starter | $2,380.00 | hypothetical |
| safety_saved | $5,000.00 | hypothetical |

## Reserve record (all hypothetical)

- exists: true — "M1 Finance", $5,000.00 (fixture stipulation; provenance `hypothetical`)
- shield_target: starter → target amount $2,380.00

## Derived figures (hand-computed, hypothetical)

- raw_safe = 1900.00 + 2688.39 − 497.39 − 130.00 = **$3,961.00**
- reserve_floor_buffer = max(0, min(3961.00, 2400.00 − 5000.00)) = **$0.00** (reserve $5,000.00 ≥ one month of essentials $2,400.00 → healthy → no buffer)
- safe_to_spend = 3961.00 − 0.00 = **$3,961.00**
- Shield: fill = 5000.00 ÷ 2380.00 = 2.1008 → capped at **100%**; level **shielded** (reserve ≥ target); bar 10 filled (bar capped at 10 characters — see report-v02.md finding F2)

## Event set (11 events, all 6:00 AM–12:00 PM America/Detroit)

---

### Event 1 — daily balance, MON SEP 21 (as_of → flamingo, colorId 4)

- Title: `$1,900.00 balance`
- Color: 4 (flamingo)

Notes:

```
$3,961.00 safe to spend
SAFE TILL SEP 30 — $3,961.00 safe
Safety target (starter floor): $2,380.00 — covered
Shield: 🛡️ shielded — $5,000.00 of $2,380.00 starter target (M1 Finance)
▓▓▓▓▓▓▓▓▓▓ 100%

Current: $1,900.00
Available: $1,860.00

Upcoming bills:

SEP 24 — $130.00  Roth IRA transfer

SEP 25 — $497.39  Huntington car payment

Next paycheck:

SEP 28 — $2,688.39  Paycheck

Month-to-date spending:

$210.00  Other living costs (6 charges)

$129.85  Fast food (non-work) (3 charges)

$86.50  Fuel (2 charges)

$42.30  Amazon (1 charge)

$33.25  Work food (4 charges)

$0.00  Finance (investment transfers) (0 charges)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

──────────────

$501.90  Total

Watch: unknown merchant — $78.44 (unrecognized merchant, verify)
```

---

### Event 2 — daily balance, TUE SEP 22 (grey, colorId 8)

- Title: `$1,900.00 balance`
- Color: 8 (graphite)

Notes: identical body to Event 1 (per `note_scope`, every daily balance event carries the full structure).

```
$3,961.00 safe to spend
SAFE TILL SEP 30 — $3,961.00 safe
Safety target (starter floor): $2,380.00 — covered
Shield: 🛡️ shielded — $5,000.00 of $2,380.00 starter target (M1 Finance)
▓▓▓▓▓▓▓▓▓▓ 100%

Current: $1,900.00
Available: $1,860.00

Upcoming bills:

SEP 24 — $130.00  Roth IRA transfer

SEP 25 — $497.39  Huntington car payment

Next paycheck:

SEP 28 — $2,688.39  Paycheck

Month-to-date spending:

$210.00  Other living costs (6 charges)

$129.85  Fast food (non-work) (3 charges)

$86.50  Fuel (2 charges)

$42.30  Amazon (1 charge)

$33.25  Work food (4 charges)

$0.00  Finance (investment transfers) (0 charges)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

──────────────

$501.90  Total

Watch: unknown merchant — $78.44 (unrecognized merchant, verify)
```

---

### Event 3 — daily balance, WED SEP 23 (grey, colorId 8)

- Title: `$1,900.00 balance`
- Color: 8 (graphite)

Notes: identical body to Event 1.

```
$3,961.00 safe to spend
SAFE TILL SEP 30 — $3,961.00 safe
Safety target (starter floor): $2,380.00 — covered
Shield: 🛡️ shielded — $5,000.00 of $2,380.00 starter target (M1 Finance)
▓▓▓▓▓▓▓▓▓▓ 100%

Current: $1,900.00
Available: $1,860.00

Upcoming bills:

SEP 24 — $130.00  Roth IRA transfer

SEP 25 — $497.39  Huntington car payment

Next paycheck:

SEP 28 — $2,688.39  Paycheck

Month-to-date spending:

$210.00  Other living costs (6 charges)

$129.85  Fast food (non-work) (3 charges)

$86.50  Fuel (2 charges)

$42.30  Amazon (1 charge)

$33.25  Work food (4 charges)

$0.00  Finance (investment transfers) (0 charges)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

──────────────

$501.90  Total

Watch: unknown merchant — $78.44 (unrecognized merchant, verify)
```

---

### Event 4 — daily balance, THU SEP 24 (grey, colorId 8)

- Title: `$1,900.00 balance`
- Color: 8 (graphite)

Notes: identical to Event 1, except the Roth IRA transfer is due this day → ` (due today)` appended per `upcoming_bill_row`. The fixture stipulates it is not yet posted, so it stays listed.

```
$3,961.00 safe to spend
SAFE TILL SEP 30 — $3,961.00 safe
Safety target (starter floor): $2,380.00 — covered
Shield: 🛡️ shielded — $5,000.00 of $2,380.00 starter target (M1 Finance)
▓▓▓▓▓▓▓▓▓▓ 100%

Current: $1,900.00
Available: $1,860.00

Upcoming bills:

SEP 24 — $130.00  Roth IRA transfer (due today)

SEP 25 — $497.39  Huntington car payment

Next paycheck:

SEP 28 — $2,688.39  Paycheck

Month-to-date spending:

$210.00  Other living costs (6 charges)

$129.85  Fast food (non-work) (3 charges)

$86.50  Fuel (2 charges)

$42.30  Amazon (1 charge)

$33.25  Work food (4 charges)

$0.00  Finance (investment transfers) (0 charges)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

──────────────

$501.90  Total

Watch: unknown merchant — $78.44 (unrecognized merchant, verify)
```

---

### Event 5 — daily balance, FRI SEP 25 (grey, colorId 8)

- Title: `$1,900.00 balance`
- Color: 8 (graphite)

Notes: identical to Event 1, except the car payment is due this day → ` (due today)` appended. Not yet posted (fixture stipulation), so it stays listed and stays subtracted.

```
$3,961.00 safe to spend
SAFE TILL SEP 30 — $3,961.00 safe
Safety target (starter floor): $2,380.00 — covered
Shield: 🛡️ shielded — $5,000.00 of $2,380.00 starter target (M1 Finance)
▓▓▓▓▓▓▓▓▓▓ 100%

Current: $1,900.00
Available: $1,860.00

Upcoming bills:

SEP 24 — $130.00  Roth IRA transfer

SEP 25 — $497.39  Huntington car payment (due today)

Next paycheck:

SEP 28 — $2,688.39  Paycheck

Month-to-date spending:

$210.00  Other living costs (6 charges)

$129.85  Fast food (non-work) (3 charges)

$86.50  Fuel (2 charges)

$42.30  Amazon (1 charge)

$33.25  Work food (4 charges)

$0.00  Finance (investment transfers) (0 charges)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

──────────────

$501.90  Total

Watch: unknown merchant — $78.44 (unrecognized merchant, verify)
```

---

### Event 6 — daily balance, SAT SEP 26 (grey, colorId 8)

- Title: `$1,900.00 balance`
- Color: 8 (graphite)

Notes: identical body to Event 1.

```
$3,961.00 safe to spend
SAFE TILL SEP 30 — $3,961.00 safe
Safety target (starter floor): $2,380.00 — covered
Shield: 🛡️ shielded — $5,000.00 of $2,380.00 starter target (M1 Finance)
▓▓▓▓▓▓▓▓▓▓ 100%

Current: $1,900.00
Available: $1,860.00

Upcoming bills:

SEP 24 — $130.00  Roth IRA transfer

SEP 25 — $497.39  Huntington car payment

Next paycheck:

SEP 28 — $2,688.39  Paycheck

Month-to-date spending:

$210.00  Other living costs (6 charges)

$129.85  Fast food (non-work) (3 charges)

$86.50  Fuel (2 charges)

$42.30  Amazon (1 charge)

$33.25  Work food (4 charges)

$0.00  Finance (investment transfers) (0 charges)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

──────────────

$501.90  Total

Watch: unknown merchant — $78.44 (unrecognized merchant, verify)
```

---

### Event 7 — daily balance, SUN SEP 27 (grey, colorId 8)

- Title: `$1,900.00 balance`
- Color: 8 (graphite)

Notes: identical body to Event 1.

```
$3,961.00 safe to spend
SAFE TILL SEP 30 — $3,961.00 safe
Safety target (starter floor): $2,380.00 — covered
Shield: 🛡️ shielded — $5,000.00 of $2,380.00 starter target (M1 Finance)
▓▓▓▓▓▓▓▓▓▓ 100%

Current: $1,900.00
Available: $1,860.00

Upcoming bills:

SEP 24 — $130.00  Roth IRA transfer

SEP 25 — $497.39  Huntington car payment

Next paycheck:

SEP 28 — $2,688.39  Paycheck

Month-to-date spending:

$210.00  Other living costs (6 charges)

$129.85  Fast food (non-work) (3 charges)

$86.50  Fuel (2 charges)

$42.30  Amazon (1 charge)

$33.25  Work food (4 charges)

$0.00  Finance (investment transfers) (0 charges)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

──────────────

$501.90  Total

Watch: unknown merchant — $78.44 (unrecognized merchant, verify)
```

---

### Event 8 — weekly food report, SUN SEP 27 (pending, banana, colorId 5)

Week MON SEP 21 → SUN SEP 27. The fixture's today is Monday morning, so the stipulated week-to-date total is $0.00 (hypothetical).

- Title: `-$0.00 Food report (pending)`
- Color: 5 (banana)

Notes (no safety-target line in the weekly note):

```
$0.00  Groceries (0 charges)

$0.00  Dining out (0 charges)

$0.00  Coffee (0 charges)

$0.00  Work food (0 charges)

──────────────

$0.00  Total

SAFE TILL SEP 30 — $3,961.00 safe
```

---

### Event 9 — bill, THU SEP 24 (peacock, colorId 7)

- Title: `$130.00 Roth IRA transfer`
- Color: 7 (peacock)

Notes:

```
Due: THU SEP 24
Monthly — Roth IRA transfer
Amount: $130.00 (hypothetical)
```

---

### Event 10 — bill, FRI SEP 25 (peacock, colorId 7)

- Title: `$497.39 Huntington car payment`
- Color: 7 (peacock)

Notes:

```
Due: FRI SEP 25
Monthly — car payment
Amount: $497.39 (hypothetical)
```

---

### Event 11 — paycheck, MON SEP 28 (sage, colorId 2)

- Title: `$2,688.39 Paycheck`
- Color: 2 (sage)

Notes:

```
Arrives: MON SEP 28
Biweekly
Amount: $2,688.39 (hypothetical)
```
