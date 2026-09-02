# Product discovery agent — design notes

Built 2026-08-31 from the six spec files in this folder. Those originals are
kept as the design record; the loadable version lives at:

```
Product/.claude/skills/product-discovery/SKILL.md
Product/.claude/skills/product-discovery/references/jira-map.md
Product/.claude/skills/product-discovery/references/competitors.md
Product/.claude/skills/product-discovery/references/report-template.md
Product/.claude/agents/jira-auditor.md
Product/.claude/agents/prd-analyst.md
Product/.claude/agents/market-scout.md
```

Shape: **skill orchestrating three subagents.** Subagents cannot ask questions
mid-run, so the conversational layer has to be the skill; the three workers are
pure retrieval-and-distil, which is what keeps raw Jira and web content out of
the main window.

## What was wrong with the input specs

Recorded because these are the failure modes worth watching for next time.

1. **Written for the wrong harness.** Tools were `view_file`, `grep_search`,
   `find_by_name`, `read_url_content`, `search_web`, `write_to_file` —
   Windsurf/Cascade names. Claude Code uses `Read`, `Grep`, `Glob`, `WebSearch`,
   `WebFetch`, `Write`. Consequence: `jira-auditor` had **no Jira access at
   all** — filesystem tools only — so both data-driven pillars would have
   returned empty or fabricated output. Fixed by giving it the Atlassian MCP
   tools directly.
2. **Hallucinated product-line meanings, baked into the spec.**
   `AF (Auto Finance)`, `PF (Personal Finance)`, `UCL (Unsecured Lending)`.
   `PRODUCT_CONTEXT.md` says AF = AmeriFirst, UCL = FNBO, and Skeps is POS
   home-improvement financing. Every report would have inherited this. The
   correct expansions are now pinned in the skill and in `prd-analyst`.
3. **Jira topology didn't match the instance.** The spec had AF/PF/UCL/CP/
   HomePro/FCC/TIC/TGUC as PMT epics. Verified against 37 live projects: the
   epics do exist in `PMT`, but CP, FNBO, HP, SSFCC, TF, NF and OF are *also*
   separate projects, and `AF`/`PF`/`UCL`/`TIC` have no project of their own.
   Real epic keys now in `jira-map.md`.
4. **Status normalization map was mostly fictional.** `Testing Completed`,
   `Released`, `Closed`, `In Review` don't exist in `PMT`. Worse:
   - `Requirment Completed` is **misspelled in Jira** — the spec's correct
     spelling matches zero tickets. A silent-zero bug: no error, just a
     wrong answer.
   - `Recommended` is the largest bucket (25 of 100) and the spec never
     mentioned it. Given its meaning is unconfirmed, it now gets its own
     **Proposed** bucket rather than being folded into a wrong one.
   - `Tech Handover Done` is statusCategory *In Progress*; the spec filed
     `Tech Handover` under Backlog/Planned — opposite ends of the lifecycle.
   - `Blocked`, `Abandoned`, `Analysis Started` were unmapped. `Blocked` is now
     always surfaced — same defect as the project-manager writeup, where the
     agent was told to hide its worst news.
5. **`prd-analyst` read files that don't exist** — `confluence_index.md`,
   `toc.md`, `docs/prds/*`. The real index is the 12 group files under
   `confluence prds/research/groups/`; PRDs are `{page_id}_{title}.html`.
6. **Pillar 1 had no source of truth** — the same defect the best-practices
   rubric calls out as the single biggest one. Resolved as a labelled proxy
   (see below), not by inventing numbers.
7. **Mechanical:** stale model IDs (`claude-3-5-sonnet`/`haiku`), ~40 stray
   `[cite: 2]` markers, leftover ` ```markdown ` wrappers and `### 4.` headers
   from a chat paste, filename typo `discovery-orchsetrator.md`, and the
   orchestrator file truncated mid-mermaid at line 74 (sections 4+ missing).

## Decisions taken

- **Pillar 1 is a labelled proxy.** The `*_pipe_review.xlsx` files were the
  candidate funding source but turned out to be Conduent↔Skeps↔ILMAST field
  mapping specs — COBOL picture clauses, pipe positions, zero metrics. Pillar 1
  now uses `SH` defect volume and `IM` incident clusters as a proxy for
  conversion leakage, labelled as a proxy on every use, with
  `UNKNOWN: funding/conversion impact` printed wherever real numbers are needed.
- **Competitors tiered, not flat.** Ten names won't fit one subagent's ~1k
  output budget at 5 pages each. Tier 1 (ChargeAfter, Jifiti, Momnt) always
  runs; one topic-matched tier is added; skipped tiers are named in the report
  so a gap never reads as a clean result.
- **Read-only against Atlassian.** No writes, comments, or transitions. The one
  permitted write is the report file under `Analysis/`.
- **Absolute paths throughout,** because cwd is ambiguous between `Public/` and
  `Public/Product/`.
- `memory` and `effort` were checked against the subagent docs and are valid
  frontmatter — kept from the original specs.

## Still open

- **`Recommended` status semantics.** 25% of PMT tickets. Until confirmed it
  stays in its own bucket and every report prints the UNKNOWN line.
- **No funding or conversion data source exists.** Pillar 1 stays a proxy until
  one is wired in. This is the highest-value gap to close.
- **No eval.** Per the rubric, next step is 3–5 golden cases: a product line
  where the recommendations are already known, to run after every prompt edit.
- Whether discovery should auto-hand off to `DISCUSSION_AGENT` or stop at the
  recommendation. Currently it stops and names the handoff.
