# Best practices for building agents

Evaluation of `agents/project manager/writeup.md` (read alongside `readme.md`), with the general lessons that apply to every agent in this folder. Dated 2026-08-20.

## Premise problems (these outrank everything else)

- **It's a job description, not an agent spec.** It says *what a PM is responsible for*, never *what the agent does on a given invocation*. There's no trigger → inputs → actions → output → done-condition contract anywhere. Anthropic calls this the "right altitude" failure — the doc sits at the vague end ("use your judgement to create a concise but comprehensive update"), so the agent will invent a different structure every run.
- **Most of this work isn't agent-shaped.** Pull filters → diff dates against plan → render report → post. That's a *workflow*: deterministic, scheduled, predictable. Anthropic's guidance is explicit — workflows for well-defined tasks, agents only "when flexibility and model-driven decision-making are needed at scale." Reserve the agent for the judgment layer (*why* is this slipping, what's at risk), not the retrieval-and-format layer.
- **No definition of success, so no way to know it's working.** Nothing states what a correct status report contains or what "on track" means numerically. Without task-level success criteria you can't detect drift, compare prompt versions, or debug a bad run.
- **readme.md and the writeup have already diverged.** The readme wants morning/afternoon/evening checkpoints, a calendar-sourced timeline, and client-presentable artifacts. The writeup contains none of these. Also two dirs — `project manager/` and `project manager agent/` (the latter empty). Pick one.

## Hallucination risk — high, and concentrated in one place

- **The SDLC dates have no source of truth. This is the single biggest defect.** You ask it to track requirement handover, dev completion, testing completion, UAT handover, and prod release dates — and never say where those dates live. No file, no Jira field name, no Confluence page, no calendar. The agent will state "UAT starts Aug 26" with total confidence, derived from nothing.
- **"Behind or ahead of schedule" needs a baseline you haven't stored.** Ahead of *what*? Without planned-vs-actual persisted somewhere, every delay judgment is fabricated. Jira status alone cannot produce it.
- **Opaque tool handles.** `filter=13669`, `filter=10428`, `filter=13917` — the agent cannot know what's inside them, and saved filters mutate silently. "Release tickets having specific labels" never names the labels. Fix: inline the JQL, name the labels, and list the exact fields it may read (`status`, `duedate`, `fixVersion`, `assignee`).
- **No uncertainty clause, no citation rule.** For a leadership-facing agent this is the cheapest, highest-value fix: every factual claim carries a ticket key or link; missing data prints `unknown — needs input from <owner>` rather than a guess. Anthropic's hallucination guidance is exactly this — permit "I don't know", ground claims in quotes, restrict to provided documents.
- **Slack as an input is a hallucination amplifier**, not a mitigation — unstructured chatter read as fact. Scope it hard (named channels, last N hours, keyword-filtered) or drop it from the default path.
- **Logic bug, not just an omission:** you tell it to ignore `Blocked` tickets. Blocked tickets are precisely what a PM must escalate. As written, the agent is instructed to hide your worst news.

## Role understanding — genuinely ambiguous

- **Project vs Program vs Product Manager are used interchangeably.** The agent is titled Project Manager; the SDLC section says the *Program* manager coordinates all stages and owns requirement gathering and UAT; Product Manager owns grooming and handover. The agent cannot tell which hat is its own, and cannot tell whether "Product Manager" means *you* (its principal) or a third party to chase.
- **"3 releases at a given time" will be misread.** "release 1 / release 2 / release 3" reads as three named releases, but you mean three *cycle stages* across releases in flight (N in dev, N+1 in handover, N+2 in requirement-gathering). Rewrite as: each release in flight sits in one stage; expect ~3 concurrent.
- **No stakeholder registry.** You name roles (QA leads, ISG, DevOps, EMs, client) but zero people, handles, or escalation order. "Automate communication wherever possible" is unexecutable — it doesn't know whom to ping. Highest leverage per unit of effort of anything on this list.
- **No output contracts.** "Publish status reports to stakeholders," "publish weekly progress report" — no audience split, length, format, or cadence. Leadership summary ≠ channel update ≠ client UAT note. Literal templates fix this and directly serve the consistency goal from readme problem 1.
- **Read/write boundary undefined.** May it transition tickets? Comment on Jira? Post to Slack, or draft only? Default must be draft-then-approve, enforced through the tool allowlist, not through hope.
- **No numeric thresholds.** What is "behind"? 1 day, 3 days, past due date? "Flag if something isn't covered" has no definition of coverage. Absent numbers, every report is a vibe.

## Efficiency

- **Re-deriving state from raw Jira every run will bloat context and degrade recall** — context rot is real and measurable as the window grows. Anthropic's prescription is just-in-time retrieval via lightweight identifiers plus structured note-taking, with sub-agents returning a distilled 1–2k-token summary.
- **Fix both problems with one artifact: a release state file** (planned date, actual date, stage, owner, blockers per release). It's the missing baseline from the hallucination section *and* the compaction mechanism. The Daily Capture Notion DB is the natural host.
- **Not loadable as a Claude Code subagent as written** — no YAML frontmatter. If that's the target: `.claude/agents/program-manager.md` with `name`, `description` (this drives delegation), a read-only `tools` allowlist, `memory: project` so release patterns accumulate, and `skills:` to preload the SDLC reference.
- **Don't run the whole thing on one expensive model.** Extraction and diffing are mechanical; only the narrative and risk call need reasoning headroom. Split it via `model:`/`effort:` on the subagent, or a workflow with cheap extraction stages.
- **Split the file into `## Instructions` (imperative, addressed to the agent) and `## Reference` (Skeps SDLC, owners, checklists).** Right now they're interleaved, which makes the reference material read as commands.

## What I'd do, in order

1. **Create the release state file** with planned vs actual dates and owners. Nothing else works without it.
2. **Write the literal output templates** + slip thresholds in days + the citation/"unknown" rule.
3. **Move recurring reporting into a scheduled workflow**; keep the agent for judgment and exception handling.
4. **Add the stakeholder registry and a draft-only tool boundary.**
5. **Build 3–5 golden cases** — a real week of Jira data with the report you'd have written by hand. That's your eval; run it after every prompt edit.

## Open questions

- Where do SDLC dates live *today* (calendar? Confluence? spreadsheet? your head)? Answer determines whether step 1 is a migration or a data-entry habit.
- Program Manager or Project Manager — which role is this agent actually replacing?
- Autonomous Slack posting, or drafting for approval?
- Are the three Jira filters stable and shareable, or personal saved filters that will rot?

## Sources

- [Building effective agents](https://www.anthropic.com/engineering/building-effective-agents) — Anthropic
- [Effective context engineering for AI agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) — Anthropic
- [Reduce hallucinations](https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/reduce-hallucinations) — Anthropic docs
- [Claude Code subagents](https://code.claude.com/docs/en/sub-agents) — Anthropic docs
- [What is agent evaluation](https://www.braintrust.dev/articles/agent-evaluation) — Braintrust
- [LLM agent evaluation metrics](https://www.confident-ai.com/blog/llm-agent-evaluation-complete-guide) — Confident AI
