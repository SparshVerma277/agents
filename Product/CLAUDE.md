# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

# Working in this folder

Product-management working folder for **Skeps** (point-of-sale financing). No code, no build, no tests — the only executable thing is the Confluence sync, and it lives *outside* this folder (`../conlfuence automation/compare_pages.py`). `../CLAUDE.md` covers the scripts and the domain shorthand; don't re-derive them here.

Four kinds of content:

- `confluence prds/` — 414 PRD HTML exports plus the research system (own `CLAUDE.md`, `SOP.md`, `agents/`, `research/groups/`).
- `Analysis/` — working documents: problem statements, solution sketches, open questions. Often extensionless and intentionally rough.
- `projects/building agents/` — design records for agents/skills, plus the rubric they are judged against.
- `.claude/skills/` — the skills Claude Code actually loads here.

`Modules/` is empty. `related artifcats/` (misspelled on disk) holds source spreadsheets.

## Skills and their design records — the two-copy rule

- **Canonical, and the only copy Claude Code loads:** `.claude/skills/<name>/SKILL.md` — `open-issues`, `rca`, invoked as `/open-issues` and `/rca`.
- **Mirror, tracked design record, read-only:** `projects/building agents/<name> agent/SKILL.md`, sitting beside a `writeup.md`.
- Edit the canonical file, never the mirror, then re-sync and verify:

```sh
cp ".claude/skills/rca/SKILL.md" "projects/building agents/rca agent/SKILL.md"
diff ".claude/skills/rca/SKILL.md" "projects/building agents/rca agent/SKILL.md"
```

- A diff means someone edited the mirror — the mirror always loses.
- `writeup.md` is design *history*. Where it conflicts with the shipped skill, the skill wins; do not "correct" a skill back toward its writeup.
- Folder naming is inconsistent: shipped agents use the ` agent` suffix (`rca agent`, `open issues agent`); newer unshipped ones do not (`conduent`, `project manager`, `portals agent`).

### Not every file in a design-record folder is a mirror

`product discovery agent/` holds each subagent spec **twice**, and only one copy is a mirror:

| Path | What it is |
|---|---|
| `product discovery agent/agents/<name>.md` | **The mirror.** Keep byte-identical to `.claude/agents/<name>.md`. |
| `product discovery agent/<name>.md` | **An early draft, design history.** Different model, different tool list, leftover `[cite: n]` artifacts. Leave it alone. |

All three of `jira-auditor`, `market-scout`, `prd-analyst` follow this. Before syncing a subagent spec, `diff` the candidate against the canonical: a *small* diff means a stale mirror to overwrite, a *wholesale* one means you are looking at the draft. Overwriting the draft destroys the design record — recoverable from git, but only if you notice.

Subagent specs live in `.claude/agents/`, not `.claude/skills/`. `references/` files (`jira-map.md`, `competitors.md`, `report-template.md`) have **no** mirror — edit the canonical only.

### mail-digest inverts the rule

`.claude/skills/mail-digest/SKILL.md` is **not** canonical. That skill executes from Sparsh's Claude account (Settings → Capabilities → Skills), where the Claude for Outlook add-in picks it up; the readable source is the Notion page 📪 `/mail-digest`. Both local copies are transcriptions, and the `.claude/skills/` one loads in Claude Code but cannot run — it calls `execute_office_js`. See `projects/building agents/mail digest agent/writeup.md`.

## House format for any agent or skill spec

`projects/building agents/best practices for building agents.md` is the standing rubric, and both shipped skills follow it:

- YAML frontmatter with `name` + `description`; the description carries the trigger phrases, since it is what drives invocation.
- A `## Contract` table first — **Trigger / Inputs / Actions / Output / Done when** — before any prose.
- Then numbered `## Instructions`. Keep imperative instructions separate from reference material; interleaving makes reference read as command.
- Named source for every factual claim; `unknown — needs input from <owner>` over a guess.
- Deterministic retrieval belongs in a workflow; reserve the agent for the judgment layer.
- Numeric thresholds and literal output templates over adjectives ("behind schedule", "concise but comprehensive" are defects).

**Skill vs subagent:** a multi-turn interview or a user-confirmation gate → skill (a subagent runs to completion and cannot ask). Fan-out retrieval worth distilling → subagent. Both shipped skills are skills for exactly that reason; the writeups record the argument.

Both shipped skills read and publish to Notion by hard-coded page ID, and both stop at a draft — they never send. Preserve that boundary in anything new.

## The research system (`confluence prds/`)

- Read `confluence prds/agents/PRODUCT_CONTEXT.md` before any product discussion touching that directory; `confluence prds/CLAUDE.md` and `SOP.md` add rules on top of this file.
- **Never bulk-load PRDs.** `research/groups/<Group>.md` is the index — group is the filename prefix (`AF`, `PF`, `UCL`, `FNBO`, `Lyon`, `Merchant Portal`, …). Read source HTML on demand, then write findings back into the group file under `## Findings`.
- This copy is maintained by hand-copy from `../conlfuence automation/confluence prds/`, which is where the sync script writes. Preserve `{page_id}_` filename prefixes — page IDs are recovered by regex.
- New PRDs follow `PRD_STRUCTURE.md`: Problem Statement, Solution, Specs, Acceptance Criteria, Success Metrics (Assumptions optional).
- Known typo: `SOP.md` says `reaser/groups/`; the directory is `research/groups/`.

## Git

- Only 15 files under `Product/` are tracked: the two skills, their mirrors and writeups, and the rubric. PRD HTML, spreadsheets, `.eml` files and `Analysis/` notes are deliberately untracked — add named paths, never `git add .`.
- A skill change is one commit covering both copies.

## Response style

- **Use bullet points, not prose paragraphs.** Nested bullets are fine; multi-sentence paragraphs are not.
- **Be brief.** No preamble, no restating the question, no summarising what you just said.
- **Keep analytical depth.** Brevity applies to packaging, not substance. Do not drop findings to shorten a response — compress them.
- Lead with the most important or most damaging point.
- Bold only the claim itself, not whole sentences.

## Analysis work

- When asked to critique a solution, challenge premises first — a flaw in the problem framing outranks a flaw in the implementation.
- State assumptions explicitly when a document leaves something undefined, rather than guessing silently.
- Flag open questions as open. Do not resolve them by assertion.
- Give a recommendation, not a survey of options.
