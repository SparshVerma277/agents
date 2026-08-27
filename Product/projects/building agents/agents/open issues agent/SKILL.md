---
name: open-issues
description: Turn the "Open issues" Notion checklist into a dated status write-up for internal stakeholders. Carries forward everything already answered on prior dated pages so the user is only asked what is new, then publishes a Notion subpage with an at-a-glance table, what closed since the last page, the decisions needed, a brief status section, and an appendix. Use when the user says "run the open issues writeup", "update the open issues page", or asks for an issues status update for leadership/stakeholders.
---

# Open issues write-up

## Contract

| | |
| --- | --- |
| **Trigger** | User asks for an open-issues status update or write-up. |
| **Inputs** | The `Open issues` Notion page checklist and its `Archive` toggle; every dated subpage under it; the user's answers to the interview. |
| **Actions** | Build carry-forward from all prior pages → diff checklist and `Archive` against the last page → confirm-or-interview per issue → write section → assemble page. |
| **Output** | A Notion subpage under `Open issues`, titled with today's date in `YYYY-MM-DD`. |
| **Done when** | Every unchecked checklist item has a section carrying a date (or an explicit "no date"), an owner (or explicit "unassigned"), and a decision line (or "none"). Every item archived since the last page has a one-line entry under `Closed since <date>`. |

## Instructions

### 1. Load state

- Fetch the `Open issues` page: `3c49bcf9-cd5d-80b4-bbeb-ea27b9aae5ec`.
  - Unchecked items outside `Archive` are the work list.
  - Items inside the `Archive` toggle are completed. They are not interviewed as open issues; they feed the `Closed since` section — see step 1b.
- Fetch **every** dated subpage under `Summaries`, newest first, and build one carry-forward record per issue.
  - **Newest page wins per field.** If `2026-08-22` records an owner but no root cause and an older page records the root cause, use both. A field is only missing if no page has it.
  - Match issues across pages by **checklist wording**, not by section heading — headings carry the date needed, and that changes run to run.
  - Do not read prior Claude Code session transcripts. Everything carried forward comes from a published page, so it is auditable and cheap to load.
- Report the diff to the user before interviewing: new items, items newly archived, items whose target date has now passed.

### 1b. Detect what closed

- **Newly archived** = sitting in `Archive` now, and listed as open on the previous dated page.
- Also treat as newly archived anything in `Archive` now that the previous page's own `Closed since` section did not list — that catches an item added straight to `Archive` without ever appearing as open.
- `Archive` carries no dates, so this diff is the only signal available. An item archived and then un-archived between two runs will be missed. If the user says something closed, take their word over the diff.

### 2. Confirm or interview, one issue at a time

Handle one issue per turn. Never batch.

**Issues with a carry-forward record** — print what you recovered, then ask one question, not ten:

```
<issue> — carried forward from <page date>:
  root cause: … | impact: … | workaround: …
  owner: … | target date: … (hotfix / release) | ticket: …
  decision: …
Anything changed?
```

- Name the source page per field when fields came from different pages.
- **"No" means publish as recovered.** Take that answer at face value — do not re-run the ten questions to double-check it.
- A recovered field reading `unknown — needs input from <owner>` is a gap, not an answer. Print it as still unknown and ask for it.
- **A target date that has already passed is never carried forward silently.** Say it has passed and ask for the replacement.

**Issues with no prior record** — the full question set:

1. **Symptom** — what does the user see or fail to do?
2. **Root cause** — if known.
3. **Impact** — who is affected, how many, any money involved.
4. **Workaround** — and does it fully cover the case, or only some of it?
5. **Client status** — raised / escalating / not yet aware.
6. **Post release** — did this escape into production through a release? Yes or no.
7. **Ticket ID.**
8. **Owner** — and the next action they hold.
9. **Target date** — and whether that is a hotfix or a scheduled release.
10. **Decision needed** — and who decides.

**Newly archived items** — one question each: what closed it. Nothing else. The ticket ID comes from the carry-forward record if there is one; ask for it only if there is not.

Write that issue's section immediately, then move on.

If the user pastes several issues at once, take them all and come back with one consolidated list of gaps.

### 3. Analyse, do not just transcribe

The transcription is the cheap half. On every issue, actively look for these and raise them in your reply to the user — not only in the document:

- **Severity mismatch.** Does the label match the consequence? "Display-only" that misattributes one party's words to another is a credibility problem. A blocked button that clears itself in five minutes is not a crisis. Say which way it cuts.
- **Code fix ≠ done.** If a defect has already produced bad records, the fix covers new activity only. The existing book needs its own decision. Always ask.
- **Procedural vs technical control.** "We asked them not to" is not a block. If exposure is held at zero by an instruction, say so and note that a breach would be silent.
- **Workaround holes.** State the condition under which the workaround fails. A workaround that assumes the customer still has the old phone fails exactly when they don't.
- **Priority driven by escalation, not impact.** If an item was deferred because the client is not pushing, name that as the reason rather than repeating "bandwidth".
- **Capacity collisions.** Flag when ≥3 items share a date, or ≥2 share one owner on one date, or a date has no release slot behind it.
- **Urgent with no date.** Any urgent item lacking a date or owner goes in Decisions needed, always.
- **External dependencies.** An item gating a client programme outranks an internal bug of similar size. Say what slips if it slips.
- **Test escapes.** If the user explains how a defect reached production, capture it — repeated patterns are worth more to stakeholders than the individual bug.
- **Metric contamination.** If a defect distorts a number that gets reported onward, caveat the number.

### 4. Grounding rules

- **Never invent** a count, date, ticket ID, owner, or root cause. Everything traces to the user's answers or a prior page.
- **Missing data prints as** `unknown — needs input from <owner>`, never as a guess or an omission.
- **Preserve uncertainty.** If the user says "I think it's platform-wide", write it as suspected and unconfirmed, and name who can confirm.
- **Quantities stay as given.** "A lot of users" is not "hundreds". If there is no count, say there is no count.

### 5. Publish

- Create a subpage under `Open issues` titled `YYYY-MM-DD`. One page per day; if today's page exists, update it rather than creating a second.
- Publish directly, no approval round-trip. Update the page after each issue so the user can watch it build.
- Order `At a glance`, `Decisions needed` and `Status` by **date needed, soonest first**. Undated urgent items go at the top. The ordering carries the severity signal — do not number the issues, because numbering churns on every reorder.
- `Closed since` has no date to order by. List those items in checklist order.
- Notion tables are real table blocks. Markdown bold renders literally inside cells, so leave `**` out of table cells. To edit a table later, fetch the page first and match the `<table>` block as stored.
- **Every dated page carries the same sections, with the same headings, in the same order** — `At a glance`, date-coverage note, `Closed since`, `Decisions needed`, `Status`, `Appendix`. Only `Closed since` and the date-coverage note may be omitted, and only when there is nothing to report. Do not rename a section, add one, or reorder them: these pages are read as a series, and a reader comparing two dates should be comparing content, not structure. `At a glance` is mandatory on every page.

## Output template

Use this structure exactly.

```
*Open issues status — internal stakeholder update. Ordered by date needed, soonest first.*

## At a glance
[table: Date needed | Issue | Context (one line) | Owner | Type]

[Capacity note, only if a collision exists.]

## Closed since <previous page date>
[One line per newly archived item: issue — what closed it, ticket ID.
 Omit the whole section if nothing closed since the last page.]

## Decisions needed
[One-line bullets. What has to be decided — nothing else.]

## Status
### <date needed> — <issue title>
**Issue**
- [2–3 bullets, no more. Symptom, impact with numbers, workaround if one exists.]

**Post release** — yes / no

**Owner and action** — [who holds it, what happens next, ticket ID.]

---

# Appendix
> Supporting detail only. Everything essential is above.

### <issue>
[Only what is NOT in the Status section: root cause mechanics, how the defect escaped,
 third-party positions, prior deferral history. Never restate the summary.]
```

Rules for the Decisions needed section:

- **One line per decision. No sub-bullets, no rationale, no consequence clause.** If a decision needs explaining, the explanation belongs in that issue's Status or appendix entry.
- **Name the owner as a neutral trailing tag, never as the subject of the sentence.** `Retail MDR — release slot for PMT-8354. owner: Manish` not `Manish still hasn't prioritised PMT-8354`. The tag is a routing label so the reader knows who to ask; the decision is the story, not the person. Use `owner: unassigned` when no one holds it.
- **Do not name a cause for the delay** — capacity, bandwidth, competing priorities, another team's silence. Even when true, it converts a status list into an argument about someone. The absence of a date is the fact; the reason is not this document's business.
- **No recommendations and no urgency language here.** This page's job is to surface open items and the facts about them. "Needs immediate attention" and "should have been done by now" are both editorial.
- The same restraint applies to the capacity note: **report the date concentration, not whose capacity is short.** "Five of nine items carry no date; three sit on 8 Sep" is the finding.

Rules for the Closed section:

- **This section is what leadership reads first.** It sits above the problem list on purpose. Keep it to one line per item — issue, the clause that closed it, ticket ID.
- **Only items archived since the previous dated page.** No cumulative list, no running total. Each page reports its own period.
- **A one-line resolution, not a status.** "Alignment reached with Conduent, process documented" not "resolved" or "closed".
- **No appendix entry for a closed item.** If a closure needs more than a line, it was not really a closure — keep it open.
- **Omit the heading entirely when nothing closed.** An empty "Closed since" section reads as no progress; absence is quieter and more honest.

Rules for the Status block:

- **`Issue` caps at three bullets.** If it needs four, the fourth belongs in the appendix. This cap is the whole point of the field — it forces the summary to stay readable.
- **`Post release` is yes/no only.** Yes means the issue escaped into production through a release. No means it was found before release, or it is not release-related at all (an alignment item, a third-party failure, a pen-test finding). If the user has not said, ask — do not infer it from the ticket prefix.
- **`Owner and action` names a person and a next step**, not a status. "Saket — hotfix on 27 Aug, `IM-2307`" not "in progress". Unassigned prints as `unassigned — needs an owner`.

Rules for the split:

- **Status is the primary document.** It must stand alone. Someone reading only the table, decisions and status has everything they need to act.
- **The appendix holds only what is not above.** No duplication — that is what makes two copies drift.
- **An issue with no extra detail gets no appendix entry.** Do not pad it for symmetry.
- Keep bullets to one sentence where possible. Bold the claim, not the sentence.
- **Bold every person's name, everywhere it appears in the write-up** — `**Saket**`, `**Nishant**`, `**Vishal**`. In `Owner and action`, in `Decisions needed` owner tags, in `Closed since`, in the appendix. A reader scanning for their own name should find it without reading the prose. This applies to people only, not to organisations (FNBO, Conduent, EXL, EY) — bolding those too would leave nothing unbolded.
- **Exception: never bold inside a table cell.** Notion renders `**` literally there, so the `Owner` column in `At a glance` stays plain text. This is the one place a name appears unbolded.

## Reference

### Notion IDs

- `Open issues` parent page: `3c49bcf9-cd5d-80b4-bbeb-ea27b9aae5ec`
- Dated subpages live directly under it, listed under the `Summaries` heading. All of them are carry-forward sources, not just the latest.
- Completed items live in the `Archive` toggle on the parent page, as checked items. Membership is the only signal — there are no close dates there.

### Audience

Internal stakeholders. Own the problems rather than describing them from outside. Do not soften a finding because it reflects on the team — a status doc that hides the worst news is worthless.

### Ticket prefixes

- `IM-` — production issues and bugs.
- `PMT-` — product tickets raised and awaiting a release slot.
- `OT-` — operation tickets: a change to production *data* rather than to code, carried out by a script. Raised when a defect has already written bad records and those records need correcting. An `OT-` is the usual companion to an `IM-` whose fix only covers new activity.

### People seen on this work

Owners: Saket, Sheel, Ankit, Mayank, Nishant. EXL liaison: Vishal. VISA refund group: Anahita, Manish, Deepankar, Prakhar.

External parties: **FNBO** (client), **Conduent** (servicing platform), **EY** (penetration testing), **360 Finance** (integration programme gated by pen-test remediation), **EXL** (partner, owns the Conduent track).

### Client and product structure

- **FNBO** is the bank — the client.
- FNBO has two products: **HIL** and **UCL**. An issue is almost always scoped to one of them, and "fixed" for one does not mean fixed for the other. Say which product a fix covers and which is still waiting.
- **`TIC`** and **`config 11`** are lender configurations, not products. A config-level hotfix leaves the other configurations exposed — name the remaining scope rather than reporting the item as fixed.

Confirm names against the current page rather than assuming this list is complete.

### Severity heuristics

Treat as urgent when any of these holds:

- The client is actively escalating.
- It gates an external programme or a third party's timeline.
- It has already produced bad records at scale — hundreds of loans or more.
- A customer can be declined or blocked on an approved application.

Per-case severity and frequency are different arguments. A high-severity defect at one case a month is a defensible deferral; say so rather than inflating it. Volume beats severity when the two conflict.
