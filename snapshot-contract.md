# Shared-Plan Snapshot Contract

**Status: PROPOSED — not implemented.** This document specifies how an
agent would request and mint an editable, privacy-safe shared-plan
snapshot link. It describes intended behavior only. Do not present
snapshots as available until norzavi.com implements this contract, and
**do not modify norzavi.com or its codebase** to implement it — this
file is a specification for the site owner's future work.

## Purpose

Let an agent turn *reviewed* outputs (figures the user has already seen
and approved) into a link the user can open, edit, and share — without
exposing raw source data. The snapshot is the shareable proof object of
the method: "here is the plan we agreed on," not "here is my bank
account."

## Design principles

- **Reviewed outputs only.** A snapshot may only be minted from figures
  the user has explicitly approved in the current session.
- **No raw source data, ever.** Snapshot payloads contain numeric
  figures and assumptions — never raw transactions, account numbers,
  names, transaction descriptions, or documents.
- **Editable by the recipient.** Opening the link loads the plan into
  norzavi.com's existing editable snapshot view; the recipient can
  adjust figures locally. Edits never write back to the minter's data.
- **Expiring and revocable.** Every snapshot carries an expiry and can
  be revoked by the minter.

## Proposed mint flow

1. The agent assembles a snapshot payload **locally** from approved
   figures and assumptions.
2. The agent shows the user exactly what the snapshot will contain and
   asks for mint approval (this is a level-3 draft action under the
   Method Kit's autonomy rules).
3. On approval, the agent calls the mint endpoint (proposed below).
4. The endpoint returns a snapshot URL, an edit token, and an expiry.
   The agent hands the URL to the user and reports what it contains.

## Proposed endpoint

`POST https://norzavi.com/api/v1/snapshots` _(proposed; not live)_

### Request payload

```json
{
  "kit_version": "0.1.0",
  "label": "Hypothetical September safety plan",
  "figures": [
    {"key": "monthly_essentials", "amount": 2400.00, "provenance": "user-entered"},
    {"key": "saved", "amount": 500.00, "provenance": "user-entered"},
    {"key": "safety_target", "amount": 2380.00, "provenance": "hypothetical"}
  ],
  "assumptions": ["starter-floor basis", "USD", "snapshot generated 2026-09-28"],
  "horizon": "2026-09-30",
  "expires_at": "2026-10-28T00:00:00Z"
}
```

Field rules:

| Field | Required | Rule |
|---|---|---|
| `kit_version` | yes | The Method Kit version that produced the payload. |
| `label` | yes | Human-readable, must contain the word "hypothetical" or "sample" when any figure is hypothetical. |
| `figures[]` | yes | Numeric figures only. Each carries `key`, `amount`, and a `provenance` label from the kit's provenance set. |
| `assumptions[]` | yes | Plain-language strings stating what the figures assume. |
| `horizon` | no | ISO date the plan runs through (e.g. month-end). |
| `expires_at` | yes | ISO timestamp; max 90 days from mint. |

Forbidden in payloads: raw transactions, account numbers, names,
transaction descriptions, document contents, file paths, credentials.

### Response

```json
{
  "snapshot_url": "https://norzavi.com/s/abc123",
  "edit_token": "et_xyz789",
  "expires_at": "2026-10-28T00:00:00Z",
  "revoke_token": "rt_qrs456"
}
```

- `snapshot_url` — public, viewable by anyone with the link; shows
  figures, provenance labels, and assumptions only.
- `edit_token` — held by the minter; required to edit or revoke.
- `revoke_token` — single-use revocation.

## Verification checklist (for the agent, before handing over the URL)

- [ ] Every figure in the payload was approved by the user in this session.
- [ ] Every figure carries a provenance label.
- [ ] No forbidden fields are present (scan the payload before sending).
- [ ] The label marks hypothetical content as hypothetical.
- [ ] The user was told the link is viewable by anyone who has it and
      when it expires.

## What the site must guarantee (implementation notes for the site owner)

- Snapshots render from the payload alone; the server stores no source
  data beyond the payload.
- Expired snapshots return a plain "expired" page, never the figures.
- Revocation takes effect immediately.
- The editable view must not offer "save to account" or any path that
  attaches the snapshot to identity — there are no accounts by design.

## Relation to the existing site

Norzavi.com already supports editable shared-plan snapshots created
through the website. This contract extends that capability to agents. If
the site's existing snapshot format differs from this proposal, the
site's format wins and this document must be revised to match — the
contract follows the implementation, not the other way around.
