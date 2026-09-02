---
name: open-issues
description: Turn the "Open issues" Notion checklist into a dated status write-up for leadership. Carries forward everything already answered on prior dated pages so the user is only asked what is new, then publishes a Notion subpage grouping every issue under Closed this period, Needs a date, Closest release and Later, each with three bullets. On request, generates an FNBO-branded HTML mail version and attaches it to that day's page for pasting into Outlook. Use when the user says "run the open issues writeup", "update the open issues page", or asks for an issues status update for leadership/stakeholders.
---

# Open issues write-up

## Contract

| | |
| --- | --- |
| **Trigger** | User asks for an open-issues status update or write-up. |
| **Inputs** | The `Open issues` Notion page checklist and its `Archive` toggle; every dated subpage under it; the user's answers to the interview. |
| **Actions** | Build carry-forward from all prior pages → diff checklist and `Archive` against the last page → confirm-or-interview per issue → write block → assemble page → ask whether to generate the mail version → attach it. |
| **Output** | A Notion subpage under `Open issues`, titled with today's date in `YYYY-MM-DD`. On request, an HTML mail version attached to that page as a downloadable file. |
| **Done when** | Every unchecked checklist item sits in one of the four groups with three bullets carrying an owner (or explicit "unassigned"). Every item archived since the last page has a one-line entry under `CLOSED THIS PERIOD`. The mail version is generated only if asked, and is attached to that day's page. |

## Instructions

### 1. Load state

- Fetch the `Open issues` page: `3c49bcf9-cd5d-80b4-bbeb-ea27b9aae5ec`.
  - Unchecked items outside `Archive` are the work list.
  - Items inside the `Archive` toggle are completed. They are not interviewed as open issues; they feed the `CLOSED THIS PERIOD` group — see step 1b.
- Fetch **every** dated subpage under `Summaries`, newest first, and build one carry-forward record per issue.
  - **Newest page wins per field.** If `2026-08-22` records an owner but no root cause and an older page records the root cause, use both. A field is only missing if no page has it.
  - Match issues across pages by **checklist wording**, not by heading — headings carry the group or the date, and both change run to run.
  - **Pages before 2026-08-27 use an older structure** — date-ordered `## Status` sections with a `Closed since` heading, and the 22 Aug page also has a `Short version`. Read them for carry-forward all the same; the fields are the same fields under different headings. Do not reformat an old page.
  - Do not read prior Claude Code session transcripts. Everything carried forward comes from a published page, so it is auditable and cheap to load.
- Report the diff to the user before interviewing: new items, items newly archived, items whose target date has now passed.

### 1b. Detect what closed

- **Newly archived** = sitting in `Archive` now, and listed as open on the previous dated page.
- Also treat as newly archived anything in `Archive` now that the previous page's own closed group did not list (`CLOSED THIS PERIOD`, or `Closed since` on pages before 2026-08-27) — that catches an item added straight to `Archive` without ever appearing as open.
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
- **Issues are grouped, not date-ordered.** Four groups, always in this order, each a **toggle heading** so the page opens as four lines instead of nine issues:

  | Group | Contains | Colour |
  | --- | --- | --- |
  | `CLOSED THIS PERIOD · <n>` | Archived since the previous page. One line each, no three-bullet block. | `green` |
  | `NEEDS A DATE · <n>` | No target date, including anything whose date passed without a replacement. | `red` |
  | `CLOSEST RELEASE — <date> · <n>` | Scheduled for the next release slot. | `blue` |
  | `LATER · <n>` | Anything beyond that release. | `gray` |

- Within a group, order by date needed where a date exists; otherwise checklist order. **Do not number issues** — numbering churns on every reorder.
- `At a glance` and `Appendix` are also toggle headings, collapsed. The table stays available without costing nine rows of scroll.
- Notion tables are real table blocks. Markdown bold renders literally inside cells, so leave `**` out of table cells. **Fetch the page immediately before editing a table** and match the `<table>` block exactly as stored — a failed `update_content` batch applies nothing, so one bad anchor silently loses every edit in the call.
- **Every dated page carries the same blocks in the same order**: banner callout, mail attachment (once generated), `Decisions needed`, `At a glance`, the four groups, `Appendix`. A group with nothing in it is omitted along with its heading. Do not rename, add or reorder: these pages are read as a series, and a reader comparing two dates should be comparing content, not structure.

### 6. Generate the mail version

The Notion page is the record. The HTML is a **subset of it** for pasting into Outlook — it will never match the page line for line, and that is deliberate.

- **Ask first, and only once the page is finalised.** Never generate unprompted: the user verifies the page content, then asks.
- Build from `references/mail-template.html`, replacing content only. The design is fixed — 680px table, cream `rgb(250,245,240)` banner, navy `rgb(37,63,112)`, per-group card colours.
- **Both logos, side by side, always** — FNBO wordmark and slice-by-fnbo, heights matched at 34px, divider between. Inline them as base64 `data:` URIs so the file is self-contained and the images survive both the Notion attachment and the paste into Outlook.
- **Include:** the three-tile stat strip (open / no date / closed since `<date>`), `Decisions needed` as one-liners, and the four groups with their three-bullet blocks.
- **Exclude:** the appendix, and the `At a glance` table. The mail closes with a line pointing back at the Notion page for detail.
- Stamp the footer with the generation time and the page it was built from, so a stale copy is identifiable.
- **Attach it to that day's page as a downloadable `<file>` block, never an `<embed>`.** An embed renders inline and makes an already-long page far worse to scroll.
- Upload via `create-file-upload` and a multipart POST — do not pass the file through `create-attachment`'s `content` parameter, which would push ~180 KB through a tool call for no benefit.
- **Regenerating replaces the existing attachment.** One mail version per page, never a version history. Notion rewrites `file-upload://` to a resolved `file://…` URL on save, so fetch the page to get the stored `<file>` tag before swapping the source.

## Output template

Use this structure exactly.

```
<callout icon="📋" color="brown_bg">
	**Open Issues Update** — FNBO
	<date> · internal status · <n> open · <n> with no date · <n> closed since <prev date>
</callout>

[Mail attachment <file> block, once generated.]

## Decisions needed
[One-line bullets. What has to be decided, plus `owner:` — nothing else.]

## At a glance {toggle="true"}
	[table: Date needed | Issue | Context (one line) | Owner | Type]

---

## CLOSED THIS PERIOD · <n> {toggle="true" color="green"}
	*Archived since the last update on <prev date>.*
	<span color="gray">TYPE · SCOPE</span>
	**<issue title>** · `<ticket>`
	- [One line: what closed it. No three-bullet block for closed items.]

## NEEDS A DATE · <n> {toggle="true" color="red"}
	*No target date currently set against these items.*
	<span color="gray">TYPE · SCOPE</span>
	**<issue title>** · `<ticket>`
	- **Issue** — [one bullet. Consequence first, then impact, then workaround. Three sentences max.]
	- **Post release** — yes / no
	- **Owner** — **<name>** · next: <one clause>

## CLOSEST RELEASE — <date> · <n> {toggle="true" color="blue"}
	*Scheduled and aligned with FNBO.*
	[same three-bullet block. Owner action is usually "nothing outstanding".]

## LATER · <n> {toggle="true" color="gray"}
	*Beyond the next release.*
	[same three-bullet block.]

---

## Appendix {toggle="true"}
	> Supporting detail only. Everything essential is above.
	### <issue>
	[Only what is NOT in the group block: root cause mechanics, how the defect escaped,
	 third-party positions, prior deferral history. Never restate the summary.]
```

Rules for the Decisions needed section:

- **One line per decision. No sub-bullets, no rationale, no consequence clause.** If a decision needs explaining, the explanation belongs in that issue's Status or appendix entry.
- **Name the owner as a neutral trailing tag, never as the subject of the sentence.** `Retail MDR — release slot for PMT-8354. owner: Manish` not `Manish still hasn't prioritised PMT-8354`. The tag is a routing label so the reader knows who to ask; the decision is the story, not the person. Use `owner: unassigned` when no one holds it.
- **Do not name a cause for the delay** — capacity, bandwidth, competing priorities, another team's silence. Even when true, it converts a status list into an argument about someone. The absence of a date is the fact; the reason is not this document's business.
- **No recommendations and no urgency language here.** This page's job is to surface open items and the facts about them. "Needs immediate attention" and "should have been done by now" are both editorial.
- The same restraint applies to the capacity note: **report the date concentration, not whose capacity is short.** "Five of nine items carry no date; three sit on 8 Sep" is the finding.

Rules for the `CLOSED THIS PERIOD` group:

- **This group is what leadership reads first.** It sits above the problem list on purpose. Keep it to one line per item — issue, the clause that closed it, ticket ID. Closed items get **one line, not the three-bullet block**.
- **Only items archived since the previous dated page.** No cumulative list, no running total. Each page reports its own period.
- **A one-line resolution, not a status.** "Alignment reached with Conduent, process documented" not "resolved" or "closed".
- **No appendix entry for a closed item.** If a closure needs more than a line, it was not really a closure — keep it open.
- **Omit the heading entirely when nothing closed.** An empty "Closed since" section reads as no progress; absence is quieter and more honest.

Rules for the issue block:

- **Exactly three bullets per issue — `Issue`, `Post release`, `Owner`. Never a fourth.** This is a hard cap, not a target. Anything that does not fit goes to the appendix or gets cut. The output is read in an email, and a fourth bullet on nine issues is nine extra lines of scrolling.
- **The `Issue` bullet is one bullet of at most three sentences.** Lead with the consequence, then impact, then the workaround. Do not split it into sub-bullets and do not restore the old two-to-three-bullet form — compressing symptom, impact and workaround into one bullet is the point.
- **The label line carries type, date and scope; the title line carries the name and ticket.** Type is one word in caps — `BUG`, `HOTFIX`, `ALIGNMENT`, `REMEDIATION`, `OPERATION`, `PMT`. Scope is the product or configuration (`HIL`, `UCL`, `config 11`) and is omitted when the issue is not scoped to one.
- **`Post release` is yes/no only.** Yes means the issue escaped into production through a release. No means it was found before release, or it is not release-related at all (an alignment item, a third-party failure, a pen-test finding). If the user has not said, ask — do not infer it from the ticket prefix.
- **`Owner` names a person and a next step**, not a status: `**Saket** · next: a fix date`, not "in progress". **The date is omitted** — the group heading already carries it, which is where much of the old verbosity went. Inside `CLOSEST RELEASE` the action is usually `nothing outstanding`, which is itself informative. Unassigned prints as `unassigned — needs an owner`.

Rules for the split:

- **Status is the primary document.** It must stand alone. Someone reading only the table, decisions and status has everything they need to act.
- **The appendix holds only what is not above.** No duplication — that is what makes two copies drift.
- **An issue with no extra detail gets no appendix entry.** Do not pad it for symmetry.
- Keep bullets to one sentence where possible. Bold the claim, not the sentence.
- **Bold every person's name, everywhere it appears in the write-up** — `**Saket**`, `**Nishant**`, `**Vishal**`. In the `Owner` bullet, in `Decisions needed` owner tags, in `CLOSED THIS PERIOD`, in the appendix. A reader scanning for their own name should find it without reading the prose. This applies to people only, not to organisations (FNBO, Conduent, EXL, EY) — bolding those too would leave nothing unbolded.
- **Exception: never bold inside a table cell.** Notion renders `**` literally there, so the `Owner` column in `At a glance` stays plain text. This is the one place a name appears unbolded.

## Reference

### Notion IDs

- `Open issues` parent page: `3c49bcf9-cd5d-80b4-bbeb-ea27b9aae5ec`
- Dated subpages live directly under it, listed under the `Summaries` heading. All of them are carry-forward sources, not just the latest.
- Completed items live in the `Archive` toggle on the parent page, as checked items. Membership is the only signal — there are no close dates there.

### Mail-version assets

- `references/mail-template.html` — the fixed design, ported from the ClarityPay release-notes mail. Placeholder markers and the per-group colour table are documented in its header comment. **Change content only, never the design.**
- `references/logo-fnbo.png` — FNBO wordmark. `references/logo-slice.png` — slice by fnbo. Both go in the header, side by side, heights matched at 34px, inlined as base64.
- Source of the design: `projects/building agents/agents/open issues agent/ClarityPay Release Notes _ 26 Aug 2026.eml`. A second reference mail, `Incident Management - 19 Open Tickets.eml`, is where the stat-tile idea comes from. Its age-based tiles (median age, oldest ticket) are **not** reproducible — the checklist holds no first-seen dates.

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
