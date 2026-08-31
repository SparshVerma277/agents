# Jira map — verified 2026-08-31

Everything here was read from the live `skeps.atlassian.net` instance.
Re-verify before trusting it after a Jira restructure.

**cloudId:** `0abc4560-5f11-439e-a579-6f6e09fc57de`

---

## Product-line epics (all live in project `PMT` — "Product Management Tasks")

There is **no** Jira project keyed `AF`, `PF`, `UCL`, or `TIC`. That work is
tracked as epics inside `PMT`. Filter by `parent`, never by label.

| Line | Epic key(s) | Epic status as of 2026-08-31 |
| --- | --- | --- |
| AF (AmeriFirst) | `PMT-987` | Problem Posted |
| PF | `PMT-2281` | Problem Posted |
| UCL | `PMT-3625` | Problem Posted |
| CP (ClarityPay) | `PMT-1738` | Blocked |
| FNBO | `PMT-17` | Blocked |
| FCC | `PMT-6122`, `PMT-5879` (MVP), `PMT-6333` (Phase 2/BAU), `PMT-5935` (UAT feedback) | Problem Posted |
| TIC | `PMT-4923`, `PMT-7188` (Ready to Fund Stipulations) | Problem Posted |
| TGUC | `PMT-2492` | Problem Posted |
| HomePro | `PMT-4102` | Problem Posted |
| NF (National Funding) | `PMT-377`, `PMT-6323` (LOC Feb), `PMT-5744` (Feb), `PMT-5672` (Nov) | Blocked / PICKED UP |
| OMF (OneMain) | `PMT-1120` | Blocked |
| HPA | `PMT-6806` | Problem Posted |
| DWH | `PMT-8096` | Problem Posted |

Platform-level epics worth knowing: `PMT-3939` Operations Dashboard,
`PMT-352` Merchant Dashboard, `PMT-351` Lender Dashboard, `PMT-348` POS,
`PMT-349` Onboarding, `PMT-539` Merchant Activation, `PMT-471` Merchant BackOps.

Abandoned — never report these as roadmap: `PMT-356` Datalake v1, `PMT-286`
GAFCO, `PMT-266` LP, `PMT-264` Enervee, `PMT-302` UC, `PMT-19` Skeps BNPL,
`PMT-14` Avant POS, `PMT-20` Avant SFC, `PMT-18` Visa BNPL, `PMT-309` FNBO App,
and the five `Modernize - *` epics (`PMT-243` … `PMT-247`).

### Labels do not work

Across all 53 `PMT` epics exactly **one** label exists: `Amerifirst`, on
`PMT-987`. Any label-based JQL returns a near-empty set. Use
`parent = PMT-987` / `parent IN (...)`.

---

## Separate Jira projects (not PMT epics)

Several lender names exist as their *own* projects as well as PMT epics.
When a line appears in both, read both and say which you used.

| Key | Name | Key | Name |
| --- | --- | --- | --- |
| `CP` | ClarityPay | `NF` | National Funding |
| `FNBO` | FNBO | `OF` | OneMain Financial |
| `HP` | HomePro Finance | `SSFCC` | FCC |
| `TF` | TGUC Financial | | |

Supporting projects: `SH` Stability Hub (defects) · `IM` Incident Management ·
`OT` Operations V2 · `IN` Infra Access & Creation · `UIX` Design ·
`QAC` Quality Assurance · `DA` Data and Analytics · `RP` Release Planner.

Ignore: `TP1`, `TPS`, `REL` (Sparsh's personal tracker), `AB`, `DT`.

---

## Status normalization — real values only

Measured from a 100-issue `PMT` sample (non-epic), 2026-08-31, with counts:

| Real status | Count | Bucket |
| --- | --- | --- |
| `Done` | 12 | **Completed** |
| `Tech Handover Done` | 15 | **In-Flight** |
| `Dev Completed` | 20 | **In-Flight** |
| `Testing Started` | 11 | **In-Flight** |
| `Dev Started` | 3 | **In-Flight** |
| `Analysis Started` | 1 | **In-Flight** |
| `Requirment Completed` | 4 | **Planned** |
| `Problem Posted` | 7 | **Planned** |
| `Recommended` | 25 | **Proposed** — own bucket, see below |
| `Blocked` | 2 | **Blocked** — own bucket, always surfaced |

Epic-only statuses: `PICKED UP` → In-Flight. `Abandoned` → excluded from all
counts and never described as roadmap.

### Traps

- **`Requirment Completed` is misspelled in Jira.** Reproduce the typo exactly
  in JQL or the clause silently matches zero issues.
- These statuses **do not exist** in `PMT` — never put them in a JQL `IN` list:
  `Testing Completed`, `Released`, `Closed`, `In Review`, `Requirement Completed`
  (correct spelling), `Tech Handover` (without "Done").
- `Tech Handover Done` sits in statusCategory *In Progress*, not Done. It means
  handover to engineering is finished, i.e. work is starting — not shipped.
- **`Recommended` is the single largest bucket (25%) and its meaning is
  unconfirmed.** Keep it in its own **Proposed** bucket. Never fold it into
  Completed, In-Flight, or Planned, and never count it as roadmap commitment.
  Print this line in every report until Sparsh confirms the semantics:
  `UNKNOWN: "Recommended" status semantics — requires input from Sparsh`
- **Never suppress `Blocked`.** Blocked items are the highest-value signal in a
  discovery report, not noise.
