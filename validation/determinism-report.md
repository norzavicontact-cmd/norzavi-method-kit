# Determinism check — validation round 3 (2026-09-28)

**Gate:** two independent agents, given byte-identical kit contents and fixture
parameters, must produce character-identical calendar artifacts.
**Result: PASS.** All figures below are hypothetical.

## Method

- The complete current contents of all six kit files
  (`method-kit.json`, `SKILL.md`, `METHOD.md`, `snapshot-contract.md`,
  `README.md`, `CHANGES.md`) were pasted verbatim into one canonical prompt,
  followed by one fixed hypothetical fixture (as_of_date 2026-09-18,
  America/Detroit, week 2026-09-14→2026-09-20) and one exact output envelope
  (11 events + the as_of day's daily report note).
- Two independent child agents were spawned with that prompt. Both messages
  were constructed from a single in-context template; the only difference
  between them was the output filename (`/tmp/det3-a.txt` vs
  `/tmp/det3-b.txt`). The agents never saw each other.
- Each agent self-checked that the pasted `method-kit.json` parsed as JSON
  and carried `kit_version: 0.1.0` before deriving anything.

## Byte comparison

- `md5sum`: both files `2966a04e222d89c76f7cf2ff8b427450`
- size: 7,577 bytes each
- `cmp`: clean — **byte-identical, exit 0**

## Independent verification (coordinator's hand computation)

Every derived figure was recomputed independently and matched both outputs:

| Day | Current balance | Safe-to-spend | MTD total |
|---|---|---|---|
| Mon 9/14 | $1,900.00 | $3,961.00 | $620.65 |
| Tue 9/15 | $1,882.25 | $3,943.25 | $638.40 |
| Wed 9/16 | $1,787.05 | $3,848.05 | $733.60 |
| Thu 9/17 | $1,720.40 | $3,781.40 | $800.25 |
| Fri 9/18 (as_of) | $1,580.65 | $3,771.65 | $940.00 |
| Sat 9/19 | $3,757.40 | $3,757.40 | $1,451.64 |
| Sun 9/20 | $3,686.80 | $3,686.80 | $1,522.24 |

- Weekly food report (pending, Sun 9/20): `-$148.35 Food report (pending)`
  (week-to-date food 9/14–9/17: $148.35), banana, SAFE TILL figure carried
  from the refresh morning's safe ($3,771.65), no safety-target line.
- Bill events: `$130.00 Roth IRA` (peacock, Due: THU SEP 17),
  `$497.39 Huntington` (peacock, Due: FRI SEP 18).
- Paycheck event: `$2,688.39 Paycheck` (sage, Arrives: FRI SEP 18).
- Watch: `Watch: none` on all 7 days — no unrecognized merchants,
  duplicates, uncategorized charges, or non-recurring charges ≥ $250.00.
- Safety target line: `Safety target (starter floor): $2,380.00 — $905.00 to go`.
- as_of day (Fri 9/18) is flamingo; all other balance days graphite.
- `=== DAILY REPORT NOTE 2026-09-18 ===` body is byte-identical to EVENT 5/11's note.
- No trailing spaces anywhere; file ends with a single newline.

## Note

An earlier attempt at this check was aborted before producing output: the
first child prompt had inadvertently introduced a non-kit placeholder field
(`pending_food_report_figure`) and abbreviated two kit documents. That
attempt was shut down immediately and is **not** counted toward this result.
