# Norzavi Method Kit

**The opinionated method layer your AI assistant executes — not another
assistant competing with it.**

Norzavi is a calendar-as-budget method: your calendar answers one
question at a glance — *what is safe to spend through the end of the
month?* — and this kit is the complete, portable template for running
it. Give these files to any capable assistant and it can instantiate
your Finance calendar, daily balance reports, and weekly/monthly
summaries with byte-level consistency, because every convention is
pinned in a machine-readable contract.

The deterministic math runs through the live Norzavi Calculator API
(`https://norzavi.com/api/v1` — stateless, no auth, USD). This kit is
the layer on top: how an assistant turns reviewed figures and API
results into a consistent calendar and reports — without rebuilding
the logic per conversation.

## What's inside

- **`method-kit.json`** — the authoritative contract (v0.2.1):
  calendar conventions (titles, 6:00 AM–12:00 PM blocks, colors), note
  layouts, SAFE TILL semantics, the reserve floor rule, the Shield
  spec, refresh rules, autonomy levels, approval boundaries, and
  provenance rules. If docs and JSON disagree, the JSON wins.
- **`SKILL.md`** — the agent skill. Extends the calculator pattern
  (math) with the method layer (calendar output, provenance
  discipline, autonomy rules, the reserve setup questions, Shield
  rendering).
- **`METHOD.md`** — the human-readable guide to the calendar-as-budget
  system. All worked figures are labeled hypothetical samples.
- **`visual-spec.md`** — the shared visual source of truth: pinned
  glyphs, the Shield (chat + web renderings), the money-flow chart,
  the icon set, and the drift rule. AI reports and the Norzavi website
  both derive from this file, so they can't drift apart.
- **`snapshot-contract.md`** — PROPOSED spec for agent-generatable
  shared-plan snapshot links. Not implemented; spec only.
- **`validation/`** — the cross-runtime portability evidence: fresh
  agents with only these files instantiate sample Finance calendar
  weeks and file adversarial ambiguity reports. Read
  `validation/determinism-report.md` for the character-identical
  determinism check.
- **`CHANGELOG.md`** — the full change history.

## Install the skill

Keep the seven core files together in one directory and give them to
your assistant. Three routes, in order of preference:

1. **Attached skill files.** If your assistant supports file
   attachments or project files, attach the kit directory (or at
   minimum `method-kit.json` + `SKILL.md`) and tell it: "Follow the
   Norzavi Method Kit for all budgeting work."
2. **Workspace skill directory.** If your assistant loads skills from
   a directory, drop the whole kit folder in as a skill. The contract
   is self-contained; the assistant reads it like documentation.
3. **HTTP-helper fallback.** The Calculator API is public and
   no-auth, but some platforms can't register a no-auth API as a
   connector. In that case the assistant calls it directly: read the
   OpenAPI description at `https://norzavi.com/api/v1/openapi.json`,
   check `GET /api/v1/capabilities`, and `POST /api/v1/calculate`
   with only the operation's numeric inputs — never names, account
   numbers, raw transactions, or documents. (This is the documented
   fallback; it works everywhere HTTP works.)

Then run setup: the skill asks what money question you're exploring,
collects figures (you type them, or name a connected source to derive
them from), asks the three reserve questions below, and confirms
everything before any API call.

## The two ideas that make it "safe"

**The reserve floor.** The method asks where your emergency savings
live — it can be more than one place (a savings account, another
platform, cash at home), and "I'm not sure of the exact amount" is a
fine answer. If no healthy reserve is on record, safe-to-spend holds
back up to one month of essentials as a starter buffer and says so
plainly: *"$0.00 safe to spend — $500.00 held back as a starter
buffer because no emergency reserve is on record."* Thin checking
balances are never mislabeled as safe. The reserve itself is never
counted as spendable — only its existence, rough size, and location
are recorded, and you report them; the method never verifies them.

**The Shield.** A two-line section in every daily report showing your
reserve as a fraction of your safety target (a starter target or a
job-loss target, your choice), with four named levels — unshielded,
building, guarded, shielded — and a fill bar derived from the
displayed percent, so the number and the picture agree by
construction. The same spec renders on the Norzavi website, so the
chat report and the site can never disagree about what your Shield
looks like.

## Philosophy

No login, no backend, local-first. Stateless math, no money movement,
approval before every API call, and every figure labeled by
provenance: user-entered, connected-source-derived, or hypothetical.
Raw transactions, account numbers, names, and documents never leave
your machine — only an operation's numeric inputs reach the public
API.

## Contributing

Issues and pull requests welcome. Two rules:

1. **Every example figure must be labeled hypothetical.** Real
   financial figures — yours or anyone's — never belong in the repo,
   in issues, or in fixtures. Validation artifacts are hypothetical
   by convention; keep it that way.
2. **Don't weaken the contract to fix an ambiguity.** If two runtimes
   diverge on the same fixture, the fix is to tighten the spec, never
   to relax a rule. See the changelog for how past findings were
   handled.

## Links

- Norzavi website: https://norzavi.com *(placeholder — kit docs live
  here; the install guide and launch announcement will be published
  at this address)*
- Calculator API docs: https://norzavi.com/api/v1/openapi.json

## License

MIT — see [LICENSE](LICENSE). (Owner's note: the license choice is
the project owner's call and trivial to change; MIT is a starting
default, not a commitment.)
