---
name: product-discovery
description: Run product discovery for a Skeps product line or capability — fans out three subagents over Jira roadmap state, existing Confluence PRDs and public competitor pages, then triangulates them into 3–5 prioritized, cited recommendations. Optionally writes a full discovery report. Use when the user asks for discovery, a roadmap gap analysis, competitor benchmarking, feature ideation, or "what should we build next" for a product line.
---

# Product discovery orchestrator

## Contract

| | |
| --- | --- |
| **Trigger** | User asks for discovery, roadmap gaps, competitor benchmarking, or feature ideation on a named line or capability. |
| **Inputs** | The topic. Then three subagent summaries — never raw tickets, PRDs, or web pages. |
| **Actions** | Scope the topic → fan out 3 subagents in parallel → triangulate 3 pillars → 3–5 recommendations → report on request. |
| **Output** | 3–5 recommendations in chat. A full report file only when asked. |
| **Done when** | Every factual claim carries a citation, every pillar with no data prints `UNKNOWN`, and the user has the recommendations in chat. |

## Step 0 — Scope before you spawn

Ask for the topic if it isn't already clear — a product line (`AF`, `PF`, `UCL`,
`FNBO`, `CP`, `TIC`, `TGUC`, `HomePro`, `NF`, `FCC`) or a capability
(funding flow, work orders, offer presentation, merchant onboarding…).

Do not run discovery across all lines at once. It produces a matrix nobody
reads and blows every token budget. One line, or one capability across lines.

Then read `/Users/sparsh/Public/Product/confluence prds/agents/PRODUCT_CONTEXT.md` yourself, once,
for the grounding — user types, lifecycle, lender list.

## Step 1 — Fan out, in one message

Spawn all three in a single message so they run concurrently:

- **`jira-auditor`** — roadmap state and defect pressure for the topic
- **`prd-analyst`** — what is already documented for the topic
- **`market-scout`** — how the competitor set handles the topic

Give each the topic and nothing else — they read their own references. Expect
~1k tokens back from each.

**Context isolation is the point.** Never pull raw Jira payloads, PRD HTML, or
web pages into your own context. If a subagent returns something too thin to
use, send it back with a sharper brief rather than doing its retrieval yourself.

If a subagent returns nothing usable, say so in the output. Do not backfill its
pillar from your own knowledge.

## Step 2 — Triangulate three pillars

**Pillar 1 — Defect pressure** *(proxy)*
Where are SH defects and IM incidents clustering for this line? Treat this as a
proxy for where conversion is leaking, and **label it as a proxy every time**.

> Skeps has no funding/conversion data source wired to this skill. The
> `*_pipe_review.xlsx` files in `/Users/sparsh/Public/` are Conduent↔ILMAST
> field-mapping specs, not metrics — do not read them for funding data.
> Whenever a recommendation would need real funding or conversion numbers,
> print: `UNKNOWN: funding/conversion impact — requires input from Sparsh`.

**Pillar 2 — Epic delta**
What is Completed vs In-Flight vs Planned vs Proposed vs Blocked for this line,
and what is missing relative to comparable lines? Blocked items are findings,
not noise — surface them.

**Pillar 3 — Competitor parity**
Which capabilities appear on competitor public pages that this line has no
epic and no PRD for? That intersection is the discovery output.

A recommendation is strongest when all three pillars point at it. Say how many
pillars support each one.

## Step 3 — Recommend

3–5 recommendations, max 3 bullets each. Lead with the highest-leverage one.

```markdown
### Product discovery — <topic>

1. **<Recommendation>**
   - **Why:** <gap or parity finding> [^citation]
   - **Pillars:** <n>/3 — <which> · **Lines:** `<AF / PF / Platform>`
   - **Impact:** <what it moves> · **Existing work:** `<epic keys or "none">`

> Sources: Jira (N issues) · PRDs (N files) · Competitors (N pages, tiers <n>)
> Uncertainty: `UNKNOWN: <point> — requires input from <owner>`
```

## Step 4 — Report, only if asked

If the user asks for a report, follow `references/report-template.md` and write
to `/Users/sparsh/Public/Product/Analysis/discovery-<topic>-<YYYY-MM-DD>.md`.

**Write boundary:** you may create that one report file. You may not modify any
PRD, group context file, `PRODUCT_CONTEXT.md`, or any Jira ticket. No Jira
writes, comments, or transitions — this skill is read-only against Atlassian.

## Grounding rules

- Every claim about internal state cites a ticket key or a PRD filename.
  Every competitor claim cites a URL. No citation, no claim.
- Missing data prints `UNKNOWN: <topic> — requires input from <owner>`. Never
  a plausible guess.
- Inferences go under `### Unverified assumptions`, not in the matrix.
- **AF = AmeriFirst. UCL = FNBO. CP = ClarityPay.** Skeps is point-of-sale
  financing, largely home improvement — not auto finance, not unsecured
  personal lending. Never expand a code beyond `PRODUCT_CONTEXT.md`.
- Almost everything is lender-configurable. For each recommendation, state
  whether it needs per-lender configuration, and if not, why not.

## Brevity

Chat output is 3–5 recommendations, 3 bullets each. Expand only on explicit
request ("deep dive", "explain", "expand"). If asked *why*, trace it:
subagent signals received → pillars applied → line comparison → recommendation.

## Handoff

Discovery ends at "here is what to build and why". It does not write PRDs.

- Idea needs pressure-testing → `/Users/sparsh/Public/Product/confluence prds/agents/DISCUSSION_AGENT.md`
- Idea is solid, needs documenting → `/Users/sparsh/Public/Product/confluence prds/agents/PRD_AGENT.md`

Close by naming which handoff fits.

## What this skill does not do

- Does not write or modify PRDs, group files, or Jira tickets
- Does not read raw tickets, PRD HTML, or web pages into its own context
- Does not report a pillar it has no data for as anything but `UNKNOWN`
- Does not run all product lines in one pass
