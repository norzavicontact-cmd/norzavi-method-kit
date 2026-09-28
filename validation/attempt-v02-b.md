# Fixture B — no reserve on record (v0.2 validation)

**All figures in this file are hypothetical.** They are the fixture's stipulated actuals (per `drafting_and_fixtures.fixture_actuals`); every figure carries provenance `hypothetical`. Hand arithmetic was used under the kit's hand-arithmetic exception (`validation.hand_arithmetic_exception`); production runs go through the Calculator API.

## Fixture metadata

- `as_of_date`: 2026-09-25 (the fixture's "today"; flamingo)
- `timezone`: America/Detroit (stipulated fixture parameter)
- Week: MON 2026-09-21 → SUN 2026-09-27
- `shield_target`: starter

## Fixture figures (all hypothetical)

| key | amount | provenance |
|---|---|---|
| checking_current | $500.00 | hypothetical |
| checking_available | $480.00 | hypothetical |
| monthly_essentials | $2,400.00 | hypothetical |
| safety_target_starter | $2,380.00 | hypothetical |
| safety_saved | $0.00 | hypothetical |

No in-horizon bills, paychecks, or transfers at all (fixture stipulation). The task's generic envelope mentions 2 bill events + 1 paycheck event, but the fixture's explicit "none" stipulation governs, so this envelope has **8 events** (7 daily + 1 weekly) and no bill or paycheck events.

## Reserve record (all hypothetical)

- No reserve on record (exists: false, amount $0.00)

## Derived figures (hand-computed, hypothetical)

- raw_safe = 500.00 + 0.00 − 0.00 − 0.00 = **$500.00**
- reserve_floor_buffer = max(0, min(500.00, 2400.00 − 0.00)) = **$500.00** (full holdback: no reserve on record)
- safe_to_spend = 500.00 − 500.00 = **$0.00**
- Line 1 MUST carry the exact no-reserve statement (buffer > 0)
- Shield: fill = 0.00 ÷ 2380.00 = 0 → **0%**; level **unshielded**; no-reserve rendering

## Event set (8 events, all 6:00 AM–12:00 PM America/Detroit)

---

### Event 1 — daily balance, MON SEP 21 (grey, colorId 8)

- Title: `$500.00 balance`
- Color: 8 (graphite)

Notes:

```
$0.00 safe to spend — $500.00 held back as a starter buffer because no emergency reserve is on record.
SAFE TILL SEP 30 — $0.00 safe
Safety target (starter floor): $2,380.00 — $2,380.00 to go
Shield: 🛡️ unshielded — no emergency reserve on record
░░░░░░░░░░ 0%

Current: $500.00
Available: $480.00

Upcoming bills:

none

Next paycheck:

none

Month-to-date spending:

$96.20  Other living costs (4 charges)

$54.10  Fast food (non-work) (2 charges)

$41.75  Fuel (1 charge)

$0.00  Finance (investment transfers) (0 charges)

$0.00  Work food (0 charges)

$0.00  Amazon (0 charges)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

──────────────

$192.05  Total

Watch: none
```

---

### Event 2 — daily balance, TUE SEP 22 (grey, colorId 8)

- Title: `$500.00 balance`
- Color: 8 (graphite)

Notes: identical body to Event 1.

```
$0.00 safe to spend — $500.00 held back as a starter buffer because no emergency reserve is on record.
SAFE TILL SEP 30 — $0.00 safe
Safety target (starter floor): $2,380.00 — $2,380.00 to go
Shield: 🛡️ unshielded — no emergency reserve on record
░░░░░░░░░░ 0%

Current: $500.00
Available: $480.00

Upcoming bills:

none

Next paycheck:

none

Month-to-date spending:

$96.20  Other living costs (4 charges)

$54.10  Fast food (non-work) (2 charges)

$41.75  Fuel (1 charge)

$0.00  Finance (investment transfers) (0 charges)

$0.00  Work food (0 charges)

$0.00  Amazon (0 charges)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

──────────────

$192.05  Total

Watch: none
```

---

### Event 3 — daily balance, WED SEP 23 (grey, colorId 8)

- Title: `$500.00 balance`
- Color: 8 (graphite)

Notes: identical body to Event 1.

```
$0.00 safe to spend — $500.00 held back as a starter buffer because no emergency reserve is on record.
SAFE TILL SEP 30 — $0.00 safe
Safety target (starter floor): $2,380.00 — $2,380.00 to go
Shield: 🛡️ unshielded — no emergency reserve on record
░░░░░░░░░░ 0%

Current: $500.00
Available: $480.00

Upcoming bills:

none

Next paycheck:

none

Month-to-date spending:

$96.20  Other living costs (4 charges)

$54.10  Fast food (non-work) (2 charges)

$41.75  Fuel (1 charge)

$0.00  Finance (investment transfers) (0 charges)

$0.00  Work food (0 charges)

$0.00  Amazon (0 charges)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

──────────────

$192.05  Total

Watch: none
```

---

### Event 4 — daily balance, THU SEP 24 (grey, colorId 8)

- Title: `$500.00 balance`
- Color: 8 (graphite)

Notes: identical body to Event 1.

```
$0.00 safe to spend — $500.00 held back as a starter buffer because no emergency reserve is on record.
SAFE TILL SEP 30 — $0.00 safe
Safety target (starter floor): $2,380.00 — $2,380.00 to go
Shield: 🛡️ unshielded — no emergency reserve on record
░░░░░░░░░░ 0%

Current: $500.00
Available: $480.00

Upcoming bills:

none

Next paycheck:

none

Month-to-date spending:

$96.20  Other living costs (4 charges)

$54.10  Fast food (non-work) (2 charges)

$41.75  Fuel (1 charge)

$0.00  Finance (investment transfers) (0 charges)

$0.00  Work food (0 charges)

$0.00  Amazon (0 charges)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

──────────────

$192.05  Total

Watch: none
```

---

### Event 5 — daily balance, FRI SEP 25 (as_of → flamingo, colorId 4)

- Title: `$500.00 balance`
- Color: 4 (flamingo)

Notes: identical body to Event 1.

```
$0.00 safe to spend — $500.00 held back as a starter buffer because no emergency reserve is on record.
SAFE TILL SEP 30 — $0.00 safe
Safety target (starter floor): $2,380.00 — $2,380.00 to go
Shield: 🛡️ unshielded — no emergency reserve on record
░░░░░░░░░░ 0%

Current: $500.00
Available: $480.00

Upcoming bills:

none

Next paycheck:

none

Month-to-date spending:

$96.20  Other living costs (4 charges)

$54.10  Fast food (non-work) (2 charges)

$41.75  Fuel (1 charge)

$0.00  Finance (investment transfers) (0 charges)

$0.00  Work food (0 charges)

$0.00  Amazon (0 charges)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

──────────────

$192.05  Total

Watch: none
```

---

### Event 6 — daily balance, SAT SEP 26 (grey, colorId 8)

- Title: `$500.00 balance`
- Color: 8 (graphite)

Notes: identical body to Event 1.

```
$0.00 safe to spend — $500.00 held back as a starter buffer because no emergency reserve is on record.
SAFE TILL SEP 30 — $0.00 safe
Safety target (starter floor): $2,380.00 — $2,380.00 to go
Shield: 🛡️ unshielded — no emergency reserve on record
░░░░░░░░░░ 0%

Current: $500.00
Available: $480.00

Upcoming bills:

none

Next paycheck:

none

Month-to-date spending:

$96.20  Other living costs (4 charges)

$54.10  Fast food (non-work) (2 charges)

$41.75  Fuel (1 charge)

$0.00  Finance (investment transfers) (0 charges)

$0.00  Work food (0 charges)

$0.00  Amazon (0 charges)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

──────────────

$192.05  Total

Watch: none
```

---

### Event 7 — daily balance, SUN SEP 27 (grey, colorId 8)

- Title: `$500.00 balance`
- Color: 8 (graphite)

Notes: identical body to Event 1.

```
$0.00 safe to spend — $500.00 held back as a starter buffer because no emergency reserve is on record.
SAFE TILL SEP 30 — $0.00 safe
Safety target (starter floor): $2,380.00 — $2,380.00 to go
Shield: 🛡️ unshielded — no emergency reserve on record
░░░░░░░░░░ 0%

Current: $500.00
Available: $480.00

Upcoming bills:

none

Next paycheck:

none

Month-to-date spending:

$96.20  Other living costs (4 charges)

$54.10  Fast food (non-work) (2 charges)

$41.75  Fuel (1 charge)

$0.00  Finance (investment transfers) (0 charges)

$0.00  Work food (0 charges)

$0.00  Amazon (0 charges)

$0.00  Car maintenance (0 charges)

$0.00  Other Apple Pay costs (0 charges)

──────────────

$192.05  Total

Watch: none
```

---

### Event 8 — weekly food report, SUN SEP 27 (pending, banana, colorId 5)

Week MON SEP 21 → SUN SEP 27. Stipulated week-to-date total: $43.55 (hypothetical).

- Title: `-$43.55 Food report (pending)`
- Color: 5 (banana)

Notes (no safety-target line in the weekly note):

```
$28.75  Dining out (2 charges)

$14.80  Groceries (1 charge)

$0.00  Coffee (0 charges)

$0.00  Work food (0 charges)

──────────────

$43.55  Total

SAFE TILL SEP 30 — $0.00 safe
```
