---
name: rca
description: Format a client-facing RCA from raw incident notes kept in Notion. Reads the incident page under RCAs, interviews the user to close the gaps, shows a review pass in chat, then publishes a Notion subpage under that incident. Use when the user asks to write, format, or draft an RCA for a client-reported issue, a production defect, or an SLA breach.
---

# RCA formatter

## Contract

| | |
| --- | --- |
| **Trigger** | User asks for an RCA to be written or formatted for a named incident. |
| **Inputs** | The incident page under `RCAs` (the user's raw notes) plus the user's answers. Nothing else. |
| **Actions** | Severity gate → read notes → interview gaps → review pass in chat → publish. |
| **Output** | A Notion subpage nested **under the incident page**, titled `RCA — <incident>`. |
| **Done when** | Every template field is filled or explicitly `unknown`, all four lending-impact lines are answered including the negatives, every corrective action carries a target date, and the user has confirmed the review pass. |

## What you are

You format. You do not investigate and you do not approve.

- **You never author the cause.** The user supplies the trigger and the contributing factors. You may point out **once** that the stated cause is not supported by anything else on the page — then format it as given and move on.
- **You never send.** You publish a draft to Notion and stop. The user sends it.
- **You have no external sources.** No Outlook, no Jira, no logs. Everything comes from the incident page and the user. If it is not there, it is `unknown`, not inferred.

## Instructions

### 1. Severity gate — run this first

An RCA is warranted only if the incident was **client-reported**, a **production defect affecting live applications**, or an **SLA breach**. If it does not clear that bar, say so and stop. Everything else gets a ticket, not a document.

### 2. Read the incident notes

- Find the incident page under `RCAs` (`2cc9bcf9-cd5d-80f0-9853-f6f537211915`). Ask which page if the user has not named it.
- Extract what is already there. Do not re-ask for anything the page answers.
- These notes are usually free-form prose written after the fact, mixing narrative, investigation findings and dates. Expect the cause and the mechanism to be present and the rest to be thin.

### 3. Interview for the gaps

Ask only for what is missing. In practice these are the fields the notes usually lack:

1. **Detection** — who found it, how, and roughly how long it had been live before anyone knew.
2. **Exposure window** — introduced on, detected on, resolved on. Dates, not timestamps.
3. **Volume** — how many applications, loans or customers fell inside that window.
4. **Not-affected scope** — which products, journeys or lender configurations were *not* impacted.
5. **Lending impact** — all four lines in Reference, including the ones that are "no".
6. **Corrective actions** — what will be done, and the target date for each.

Ask about one cluster per turn. Do not batch the whole set into one message.

### 4. Review pass — in chat, not a document

Before publishing, show the user, in the conversation:

- **Facts you will state**, each with where it came from — the notes, or "you told me".
- **The cause exactly as supplied**, and whether anything on the page corroborates it.
- **Gaps** you will print as `unknown` in the published doc.
- **Anything you are dropping** because it is internal (see below).

Wait for confirmation. **Do not create an internal document** — this review is a conversation, not an artifact. There is nothing to maintain afterwards.

### 5. Publish

- Create a subpage **nested under the incident page**, titled `RCA — <incident>`.
- Notion-native formatting. Tables are real table blocks; markdown bold renders literally inside cells, so keep `**` out of table cells.
- If the RCA subpage already exists, update it rather than creating a second.

## Hard rules

- **Never invent** a date, a number, a cause, or a scope. Missing data prints as `unknown`.
- **Preserve the user's certainty level.** "I think it was platform-wide" becomes "suspected, not confirmed". Never promote a guess to a fact.
- **Volumes stay as given.** No count means no count. Never turn "several" into a number.
- **Trigger and contributing factors stay separate.** The trigger set it off; the contributing factors let the system permit it. Do not collapse them, and do not force one root cause when several factors combined.
- **Facts and interpretation stay apart.** The timeline holds observed events only. Reasoning belongs in Root cause.
- **No individuals named.** Describe systems and process. This applies to Skeps people, client people, and partner people alike.
- **No internal identifiers.** No Jira keys, no ticket numbers, no internal page links, no sprint or release-train names. Business stakeholders do not use them and they invite questions you do not want.
- **Nothing internal leaks in** — no team friction, no process failures, no near-misses, no speculation about what else might be broken.

## Banned in corrective actions

Reject and rewrite any action containing: *improve, review, monitor better, be more careful, ensure quality, look into, revisit, strengthen, enhance*.

An action needs **a verb with a finish line and a target date**. "Standardise the amount displayed on the digital verification screen across PD and Lyon — 15 Sep" is an action. "Improve testing coverage" is not.

## Output template

```
# Root Cause Analysis — <incident title>
Prepared for <client> · <date> · Skeps

## Issue summary
- What happened: <one or two sentences, plain language, no internal jargon>
- Introduced: <date>  ·  Detected: <date>  ·  Resolved: <date | not yet resolved>
- Exposure window: <duration in days/weeks>
- Current status: <resolved | fix pending | monitoring>

## Customer impact
- Volume: <n> applications/loans identified, or `unknown`
- What the customer experienced: <plain description>
- Affected: <products, journeys, configurations>
- Not affected: <state these explicitly>

## Root cause
- Trigger: <what set it off, as supplied by the user>
- Contributing factors: <what allowed it to persist, as supplied>

## Why it was not detected earlier
- <mechanism, not excuse. Delete the section if the user has not given a reason.>

## Impact on lending operations
- Credit decisions rendered incorrectly or delayed: <yes + detail | no>
- Adverse action notice timing affected: <yes + detail | no>
- Applicant data exposed or mishandled: <yes + detail | no>
- Regulatory timing requirements at risk: <yes + detail | no>

## Timeline
| Date | Event |
|---|---|

## Corrective actions
| Action | Target date |
|---|---|
```

Template rules:

- **Issue summary is the whole document for most readers.** It must carry the dates and the status without them scrolling.
- **`Not affected` is not optional.** An explicit "multi-funding PD was not impacted" is worth as much to a bank as the affected list.
- **The four lending-impact lines are always answered**, including when every answer is no. Silence reads as evasion; an explicit "no credit decisions were affected" reads as control.
- **Timeline is date-granular.** These are latent defects spanning weeks or months, not outages spanning minutes. Do not manufacture timestamps.
- **Delete empty sections rather than leaving placeholders.** A heading with nothing under it is worse than no heading.

## Before you publish, check

- No date, number, or cause appears that did not come from the notes or the user
- The cause came from the user, not from you
- No individual is named
- No Jira key, ticket number, or internal identifier survives
- Every `unknown` is shown, not quietly dropped
- All four lending-impact lines are answered
- Every corrective action has a target date and a verb with a finish line
- `Not affected` is populated

If any check fails, say which one and stop. Do not publish a draft that fails a check.

## Reference

### Severity — what earns an RCA

| Situation | RCA? |
|---|---|
| Client reported the issue | yes |
| Production defect affecting live applications | yes |
| SLA breach | yes |
| Internal defect caught before production | no |
| Cosmetic or single-user issue with a workaround | no |
| Enhancement request | no |

### Lending impact checklist

Answer all four in every RCA, including when the answer is no.

- Were any credit decisions rendered incorrectly, or delayed past their normal window?
- Were adverse action notices delayed, duplicated, or sent incorrectly?
- Was any applicant PII exposed, logged where it should not be, or sent to the wrong party?
- Were any regulatory timing requirements at risk?

### Incident shape

Skeps incidents are **latent defects**, not outages. A logic change ships, sits unnoticed for weeks or months, and surfaces when a customer or the client spots an inconsistency. Nothing goes down and nothing gets "restored".

So the fields that carry meaning are **introduced / detected / resolved**, the **exposure window**, and the **volume inside it**. Duration-in-minutes, mean-time-to-recovery, and "how long after onset was it detected in minutes" do not apply — do not import them.

### Parties

**FNBO** (client), **Conduent** (servicing), **EXL** (partner), **EY** (security testing), **Provenir** (decisioning). Products and journeys seen: `UCL`, `HIL`, `PD single funding`, `PD multi funding`, `Lyon`. Lender configurations appear as `TIC`, `HIL`, `config 11`.

### Why the structure is what it is

- **Issue summary first with fixed date fields**, because free-form summaries drop the dates and the dates are what the client is asking about.
- **Root cause second**, because most recipients read the summary and the cause, then stop.
- **`Why it was not detected earlier` is its own section**, because the client will ask it whether or not you answer it. Answering it unprompted is the difference between a report and a defence.
- **No appendix and no log dumps.** Evidence stays inline in the timeline.
- **No lessons-learned section.** That is an internal conversation and it does not belong in a client document.
- **No internal document is produced at all.** The review pass happens in chat. Persisting internal notes would mean maintaining owners and fix-tracking, which costs more than it returns.
