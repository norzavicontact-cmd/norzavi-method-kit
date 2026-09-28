# Release notes — v0.2 (contract 0.2.1)

**Public release name: v0.2. Machine-readable contract: 0.2.1.**

v0.1.0 stays release-ready as shipped. v0.2 is behavior-changing: the
safe-to-spend formula gains a reserve floor, the daily report gains a
Shield section, and a shared visual spec is introduced.

## What changed and why

The v0.1 formula answered the wrong question: it computed liquidity
("won't overdraft before month-end"), not safety. $500 in checking
with no backup was advertised as $500 safe to spend. v0.2 asks about
the emergency reserve at setup and holds back a starter buffer when
no healthy reserve is on record.

**Reserve-aware safe-to-spend.** The skill asks three reserve
questions before generating calendar output: whether you have an
emergency/safety reserve, where it lives, and roughly how much is in
it. Everything is user-entered and never verified. With no reserve
on record, the method withholds up to one month of essentials and
says so in plain language:
*"$0.00 safe to spend — $500.00 held back as a starter buffer
because no emergency reserve is on record."*

**Flexible reserve holdings (0.2.1).** Savings don't all live in one
place. The reserve record is a holdings list: each holding names
where it is (free text — "M1 Finance", "Ally HYSA", "cash
envelope"), roughly how much is in it, and "I'm not sure of the
exact amount" is a fine answer per holding. The agent never judges
what counts as a reserve — any user-declared holding counts. No
account-type taxonomy, no verification.

**The Shield.** A two-line section in every daily report: your
reserve as a fraction of your safety target (starter or job-loss),
four named levels — unshielded, building, guarded, shielded — with a
fill bar derived from the displayed percent so the number and the
picture agree by construction.

**Shared visual spec.** `visual-spec.md` pins glyphs, the Shield
(chat + web + animation renderings), the money-flow chart
(income → bills → reserve → safe-to-spend), and the icon set. AI
reports and the Norzavi website both derive from it, so they can't
drift apart.

## Validation

- v0.2.0: one fresh validator instantiated two fixture weeks from
  the kit files alone — a healthy-reserve week and a no-reserve
  week. The no-reserve week never advertises the $500 balance as
  safe; the healthy week matches the worked example. 12 adversarial
  findings filed, all fixed (uncapped Shield bar, false no-reserve
  wording for partial reserves, flow-chart math, and nine more).
- v0.2.1: a fresh validator checked split-reserve fixtures ($3,000
  confirmed + $2,000 amount-unconfirmed): truthful
  amount-unconfirmed statement, Shield math on the confirmed sum,
  all assertions PASS.
- All validation figures are hypothetical. Full evidence in
  `validation/`.

## Boundaries (honest)

- Agent-minted shared-plan snapshots: **proposed, not implemented**
  (`snapshot-contract.md`).
- The website Shield, animation, and flow-chart visuals are
  **specified in `visual-spec.md` but not yet implemented** on
  norzavi.com.
- Local-first, no login, no backend, no paid tier. Nothing stored,
  nothing moved.
