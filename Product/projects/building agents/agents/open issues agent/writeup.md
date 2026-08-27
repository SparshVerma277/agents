# Open issues agent

Built 2026-08-22 from a live session that produced the first write-up. Evaluated against `../../best practices for building agents.md`.

Implementation: `Product/.claude/skills/open-issues/SKILL.md`, invoked as `/open-issues`. The `SKILL.md` beside this file is a symlink to it, not a copy — edit either path, there is only one file.

## Why a skill and not a subagent

- **The core mechanic is a multi-turn interview.** Subagents run autonomously and return a report; they cannot hold a back-and-forth with the user. Building this as a subagent forces bulk-dump input, which loses the thing that made the session work — being asked nine specific questions per issue surfaced gaps that a paste would have hidden.
- **The retrieval half is trivial.** Read a checklist, diff it against yesterday's page. There is no fan-out to justify agent overhead.
- **The judgment half is the product.** Severity reframing, spotting that a code fix leaves an existing book uncorrected, noticing that exposure is held by an instruction rather than a control. That belongs in the main thread where the user can push back mid-flight.

## How it satisfies the best-practices rubric

| Requirement from the eval | How this meets it |
| --- | --- |
| Trigger → inputs → actions → output → done-condition | Contract table at the top of the skill. |
| Not a job description | Every line is imperative and scoped to one invocation. |
| Named source of truth | The `Open issues` Notion page, by ID. Dated subpages are the persisted baseline. |
| No opaque tool handles | One page ID, stated inline. No saved filters, no unnamed labels. |
| Citation / "unknown" rule | Section 4. Missing data prints `unknown — needs input from <owner>`; quantities stay as given. |
| Numeric thresholds | Capacity collision at ≥3 items per date or ≥2 per owner per date. Urgency criteria listed explicitly. |
| Output contract | Literal template with a stated rule for what goes in the appendix versus the summary. |
| Read/write boundary | Publish directly to Notion, one page per day, update rather than duplicate. Chosen deliberately: the page is internal and Notion versions it. |
| Stakeholder registry | Reference section, with a note to re-confirm rather than trust the list. |
| Instructions / Reference split | Enforced by section headings. |

## Design decisions worth revisiting

- **Publish-direct rather than draft-then-approve.** Right call for an internal page. Would be wrong the moment this feeds a client-facing artifact.
- **Summary-primary, appendix-for-extras.** The first version duplicated content across a detailed section and a short one. Two copies of the same fact drift the moment one is edited. The appendix now holds only what the summary does not.
- **No issue numbering.** Ordering by date carries the severity signal, and numbering churns on every reorder.
- **Date-ordering as the severity mechanism.** Cheap and legible, but it breaks for an urgent item with no date — hence the rule that those go to the top and into Decisions needed.
- **Carry-forward reads all prior pages, not just the last one** (changed 2026-08-27). Newest page wins per field, so an issue detailed three weeks ago and untouched since keeps its detail. Deliberately excludes prior Claude Code session transcripts — they would slow every invocation and would pull unpublished chat into a published page.
- **Confirm-not-re-interview** (added 2026-08-27). A carried-forward issue gets its recovered fields printed back and one question: anything changed. One turn instead of ten. The two guards that keep this from publishing stale data: a recovered `unknown` is re-asked, and a target date that has passed is never carried silently.
- **`Closed since` is period-scoped, not cumulative** (added 2026-08-27). Each page lists only what was archived since the previous page, one line per item, placed above the problem list. No running total — a cumulative count invites reading the page as a scorecard rather than a status.

## Known gaps

- **No eval.** The best-practices doc calls for 3–5 golden cases. The 2026-08-22 page is case one; it needs a hand-written comparison to be a real baseline.
- **Carry-forward is unproven.** Still true after the 2026-08-27 rewrite — the logic has never run against a real second page. Two specific things to watch on the next run: whether issues match across pages by checklist wording (headings carry the date needed, which changes), and whether "anything changed?" actually gets a considered answer or a reflexive no.
- **Archive diff has no dates behind it.** Membership in the `Archive` toggle is the only close signal, so an item archived and un-archived between two runs is invisible, and a close date can only come from the user. First-seen dates would fix this and the age-tracking gap below at the same time.
- **No age tracking.** An item that has sat open for a month reads the same as one raised yesterday. First-seen dates would fix it and would have made "FNBO has raised this for a month" visible without being told.
- **Volume data is user-supplied.** Loan and user counts come from the interview, not from a query. Fine while numbers are small; a real weakness if the write-up starts carrying figures anyone acts on.
- **Severity heuristics are unvalidated.** The urgency criteria are drawn from one session. They will need adjusting once a case arrives that they classify wrongly.

## Open questions

- ~~Should closed items be summarised on the next page, or does the checklist's own state suffice?~~ Answered 2026-08-27: yes, summarised — one line each, period-scoped, above the problem list, because the page gets shared with leadership.
- Is there a release calendar the agent could read, so target dates attach to real slots instead of bare dates? This is the single largest accuracy gap.
- Who beyond the immediate group reads these pages? The tone is written for internal stakeholders who can absorb bad news directly.
