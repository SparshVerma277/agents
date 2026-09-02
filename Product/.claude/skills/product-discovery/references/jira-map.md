# Jira map — verified 2026-09-02

Everything here was read from the live `skeps.atlassian.net` instance.
Re-verify before trusting it after a Jira restructure.

**cloudId:** `0abc4560-5f11-439e-a579-6f6e09fc57de`

## Scope

Audit **`PMT`, `IM`, `SH`, `OT`** only. The lender-named standalone projects
(`CP`, `HP`, `FNBO`, `NF`, `OF`, `SSFCC`, `TF`) are **out of scope** and must
not appear in output — they hold merchant-config and onboarding-ops tickets,
not product delivery, and mixing them into a delivery count produces a
meaningless comparison. (Sparsh, 2026-09-02.)

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
| **HP (HomePro)** | **`PMT-6806` — summary "HPA", the live epic** | Problem Posted |
| NF (National Funding) | `PMT-377`, `PMT-6323` (LOC Feb), `PMT-5744` (Feb), `PMT-5672` (Nov) | Blocked / PICKED UP |
| OMF (OneMain) | `PMT-1120` | Blocked |
| DWH | `PMT-8096` | Problem Posted |

**HomePro has two epics and only one is live.** `PMT-6806` ("HPA") carried 30
tickets created in the 2026-07-02→2026-09-02 window, 6 of them Done.
`PMT-4102` ("HomePro") is dormant — zero children touched in the same window.
Use `PMT-6806`. Both read "Problem Posted", so **epic status tells you nothing
about which epic is live** — check child activity before choosing one.

Platform-level epics worth knowing: `PMT-3939` Operations Dashboard,
`PMT-352` Merchant Dashboard, `PMT-351` Lender Dashboard, `PMT-348` POS,
`PMT-349` Onboarding, `PMT-539` Merchant Activation, `PMT-471` Merchant BackOps.

Abandoned — never report these as roadmap: `PMT-356` Datalake v1, `PMT-286`
GAFCO, `PMT-266` LP, `PMT-264` Enervee, `PMT-302` UC, `PMT-19` Skeps BNPL,
`PMT-14` Avant POS, `PMT-20` Avant SFC, `PMT-18` Visa BNPL, `PMT-309` FNBO App,
and the five `Modernize - *` epics (`PMT-243` … `PMT-247`).

---

## How to identify a product line outside `PMT`

Different projects carry line identity differently. Using the wrong mechanism
is the single largest source of wrong counts — it produced five of six wrong
figures in the 2026-09-02 validation.

| Project | Mechanism | Verified example |
| --- | --- | --- |
| `PMT` | `parent = <epic key>` | `parent = PMT-6806` |
| `SH` | **`"epic[dropdown]" = "<value>"`** | `= ClarityPay` → 111 · `= "HomePro Australia"` → 131 |
| `IM` | **`textfields ~ "<code>"`** | `~ "CP"` → 31 · `~ "HP"` → 6 |
| `OT` | `unknown — needs input from Sparsh` | — |

- **`SH` uses a custom dropdown named `epic[dropdown]`.** Quote it exactly as
  `"epic[dropdown]"` in JQL. Its values are line names, not codes
  (`ClarityPay`, `HomePro Australia`), and they do **not** match the shorthand
  in `PRODUCT_CONTEXT.md`. Confirm the value spelling before trusting a zero.
- **`IM` does not use that field at all** — `project = IM AND
  "epic[dropdown]" is not EMPTY` returns **0** across the whole project. Match
  on `textfields ~`, which searches every text field rather than just the
  summary. `summary ~` alone undercounts badly.
- **Labels and components are unusable.** `labels` and `components` are empty
  in both `SH` and `IM`, and across all 53 `PMT` epics exactly one label
  exists (`Amerifirst`, on `PMT-987`). Any label-based JQL returns a
  near-empty set.

---

## Counting rules — delivered and in flight

### Never use `resolutiondate`

It is not consistently populated. On the `CP` project, `statusCategory = Done
AND resolutiondate` in a two-month window returned **0** while `statusCategory
= Done AND updated` in the same window returned **1**. A `resolutiondate`
filter silently reports zero delivery on a project that simply does not fill
the field.

### Delivered

`updated` inside the window **AND** a terminal status:

| Project | Terminal statuses |
| --- | --- |
| `PMT` | `statusCategory = Done`, **plus `Testing Completed`** |
| `IM` | `statusCategory = Done` |
| `SH` | `statusCategory = Done` |
| `OT` | `statusCategory = Done` |

Prefer `statusCategory = Done` over a list of status names — the name list is
long, project-specific, and still growing. `Testing Completed` is the one
name-level addition, because it sits in category *In Progress* but counts as
delivered for `PMT`.

**Done-category statuses that are not delivery.** Exclude these from delivered
counts and never describe them as roadmap; report them separately if they
matter:

`Abandoned` · `Duplicate` · `Moved` (IM) · `Changed To CR` (SH) ·
`Moved to Prod Bug PMT` · `Moved to TS Board` · `Deprecated` · `Not a Bug`

### In flight

Use an **allowlist**, never `status != Done`. A negation sweeps in statuses
like `Invalid Bug` and `Rejected`, which sit in category *In Progress* despite
being closed in practice.

| Project | In-flight statuses |
| --- | --- |
| `PMT` | `PICKED UP` · `Dev Started` · `Dev Completed` · `Testing Started` · `Testing In Review` · `Testing In Sanity` — **not** `Testing Completed`, which is terminal |
| `IM` | `Backlog` · `In Progress` |
| `SH` | `Backlog` · `Retest` · `Retest Failed` |
| `OT` | `unknown — needs input from Sparsh` |

Every bucket total must reconcile against the project total for the window.
State any unaccounted remainder rather than sampling.

---

## Status inventory — verified 2026-09-02

`cat` = Jira statusCategory: `Done`, `In Progress` (indeterminate), `To Do` (new).

**`PMT`** — `Done` (Done) · `Done Marked by PM` (Done) · `Duplicate` (Done) ·
`Abandoned` (Done) · `Testing Completed` (In Progress) · `Testing In Review`
(In Progress) · `Testing In Sanity` (In Progress) · `Testing Started`
(In Progress) · `Dev Completed` (In Progress) · `Dev Started` (In Progress) ·
`Tech Handover Done` (In Progress) · `Analysis Started` (In Progress) ·
`Requirment Completed` · `Problem Posted` · `Recommended` · `Blocked` ·
`PICKED UP` (epic-only)

**`SH`** — `Done` · `Backlog` · `Retest` (In Progress) · `Retest Failed`
(In Progress) · `Invalid Bug` (In Progress) · `Rejected` (In Progress) ·
`Changed To CR` (Done) · plus `Not a Bug`, `Deprecated`, `Deployment Issue`,
`Duplicate`

**`IM`** — `Done` · `To Do` · `In Progress` · `Backlog` · `Moved` (Done) ·
`Moved to Prod Bug PMT` · `ABANDONED`

**`OT`** — no status outside this set:
`Done`, `Backlog`, `To Do`, `In Progress`, `Blocked`, `Abandoned`,
`Not a Bug`, `Deprecated`, `Deployment Issue`, `Duplicate`,
`Moved to Prod Bug PMT`, `Moved to TS Board`, `Reopened`,
`Waiting for Support`. Which of them `OT` actually uses is unverified.

**Coverage caveat.** These lists were built by elimination
(`status not in (...)` until the remainder hit zero). `SH` still has **9**
issues in an unnamed status and the `PMT` remainder was not driven to zero.
Treat the lists as near-complete, and prefer `statusCategory` wherever a
decision would otherwise depend on an exhaustive name list.

---

## Separate Jira projects — out of scope, listed to avoid confusion

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

## Traps

- **`Requirment Completed` is misspelled in Jira.** Reproduce the typo exactly
  in JQL or the clause silently matches zero issues.
- **`Testing Completed` DOES exist in `PMT`.** An earlier version of this file
  claimed it did not. Live examples: `PMT-8175`, `PMT-8221`.
- **`Done Marked by PM` is a Done-category status** and was absent from this
  map until 2026-09-02. Any delivered count built from a name list that omits
  it undercounts. This is the reason to filter on `statusCategory`.
- These statuses do **not** exist in `PMT` — never put them in a JQL `IN`
  list: `Released`, `Closed`, `In Review`, `Requirement Completed` (correct
  spelling), `Tech Handover` (without "Done").
- `Tech Handover Done` sits in statusCategory *In Progress*, not Done. It means
  handover to engineering is finished, i.e. work is starting — not shipped.
  It is **not** in the in-flight allowlist above; see the open question.
- **`Invalid Bug` and `Rejected` (`SH`) are category *In Progress*** despite
  being closed in practice. They are the reason in-flight must be an
  allowlist rather than `status != Done`.
- **`Recommended` is the single largest `PMT` bucket (25% of a 100-issue
  sample) and its meaning is unconfirmed.** Keep it in its own **Proposed**
  bucket. Never fold it into Completed, In-Flight, or Planned, and never count
  it as roadmap commitment. Print this line in every report until Sparsh
  confirms the semantics:
  `UNKNOWN: "Recommended" status semantics — requires input from Sparsh`
- **Never suppress `Blocked`.** Blocked items are the highest-value signal in a
  discovery report, not noise.

## Open questions

- `UNKNOWN: whether "Tech Handover Done" and "Analysis Started" count as in
  flight — requires input from Sparsh.` Neither appears in the in-flight
  definition given on 2026-09-02 ("picked up, dev stages, testing stages"),
  so both are currently excluded from in-flight and from delivered, which
  leaves them in no bucket.
- `UNKNOWN: OT line-identity field and in-flight statuses — requires input
  from Sparsh.`
