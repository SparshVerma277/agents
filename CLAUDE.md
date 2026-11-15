# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

Sparsh's product-management working directory for **Skeps**, a point-of-sale financing platform. It is not an application codebase — there is no build, no test suite, no shared dependency manifest at the root. It holds three kinds of thing:

1. **Standalone Python automation scripts** (Confluence sync, Jira reporting) — each in its own folder, each self-contained.
2. **Product documentation and analysis** — Confluence PRD exports, spreadsheets, working notes.
3. **Claude Code skills and agent specs** being developed as product artifacts in their own right.

Git tracks only a curated ~21 files (skills, agent specs, learning notes). Everything else — data exports, spreadsheets, PRD HTML, credentials — is intentionally untracked. Do not `git add` broadly; add named paths.

## Automation scripts

Every script follows the same shape: a `config.json` sitting next to it holds credentials and query parameters, and the script is run with the working directory set to its own folder. **Always `cd` into the script's folder before running it** — paths like `config.json`, `page_mapping.csv`, and the `confluence prds/` output directory are all resolved relative to cwd.

| Folder | Entry point | Purpose |
|---|---|---|
| `conlfuence automation/` | `compare_pages.py` | **Current** Confluence→local PRD sync (CQL-based subpage discovery) |
| `confluence/` | `compare_pages.py`, `reverse_changelog.py` | Older sync fork; changelog table reordering |
| `change log/` | `reorder_changelogs.py` | Sorts Confluence changelog tables by date |
| `email_updates_script/` | `jira_email_report.py` | Jira → HTML email digest via SMTP/MSAL OAuth |
| `open tickets automation/` | `open_tickets_report.py` | Jira open-ticket report via SendGrid |
| `last 15 days tickets daily/` | `jira_export_transform.py`, `manage_cron.py` | Daily Jira export → Excel; cron-scheduled |
| `jira_automation/CP/`, `jira_automation/FNBO/` | `jira_sequential_ops_*.py`, `jira_bulk_create.py` | Per-lender bulk Jira create/update from CSV |

```bash
# Confluence PRD sync (the one that is current)
cd "conlfuence automation" && pip install -r requirements.txt && python compare_pages.py

# Jira email digest — dry-run prints HTML instead of sending
cd email_updates_script && python jira_email_report.py --dry-run
cd email_updates_script && python jira_email_report.py --config config.json

# Open tickets report — dry-run writes dry_run_output.html
cd "open tickets automation" && python open_tickets_report.py --dry-run

# Changelog reversal — pilot one page first
cd confluence && python3 reverse_changelog.py --page-id 918552577 --dry-run

# Daily Jira export: installs deps and registers the 1:00 PM cron job
cd "last 15 days tickets daily" && ./setup.sh
```

Scripts that write to Confluence or send mail expose `--dry-run` (or a `dry_run` flag in `config.json`, as in `change log/`). **Use it first** — `reorder_changelogs.py`, `reverse_changelog.py`, and the `jira_automation` scripts mutate live Atlassian data, and several iterate over every page matched by a CQL query rather than a single target.

Dependencies are per-folder `requirements.txt` (`atlassian-python-api`, `beautifulsoup4`, `jira`, `sendgrid`, `msal`) plus `requests`/`pandas`/`openpyxl`/`python-crontab` installed by `setup.sh`. There is no virtualenv convention; scripts are run against the system Python 3.

### config.json holds plaintext secrets

Every `config.json` in this repo contains a live Atlassian API token, and some contain SendGrid keys or OAuth client secrets. They are untracked and must stay that way. When inspecting one, print key structure rather than values.

## Confluence PRD exports

`compare_pages.py` mirrors a Confluence space into a sibling `confluence prds/` directory, one file per page named `{page_id}_{page_title}.html`. Sync is version-based: the export embeds `<meta name="confluence-version" content="N">`, and a page is re-downloaded only when the remote version number is higher. A missing meta tag (e.g. a legacy `.pdf`) forces a re-download and converts the file to `.html`. Page IDs are recovered from filenames by a `\d{6,}` regex, so **preserve the `{id}_` prefix when renaming**. Bodies are raw Confluence storage format and may contain `<ac:*>` macro tags.

**Three copies of this directory exist and they are not in sync:**

- `Product/confluence prds/` — 414 files, byte-identical to the one below, and the copy the research system points at. It has **no** `compare_pages.py` in its parent, so its own CLAUDE.md instruction to run `../compare_pages.py` does not work from there.
- `conlfuence automation/confluence prds/` — same 414 files; **this is the one the current sync script writes to.**
- `confluence/confluence prds/` — 276 files, stale since April, plus its own `GEMINI.md`.

To refresh PRDs, sync under `conlfuence automation/` and copy across if `Product/` needs to match. When reading a PRD, prefer `Product/confluence prds/`.

## The research system

`Product/confluence prds/` carries its own conventions, documented in `SOP.md` and its local `CLAUDE.md`:

- **Never load PRDs in bulk.** 414 HTML files will not fit in context. Group context files under `research/groups/` (`AF.md`, `PF.md`, `UCL.md`, `FNBO.md`, `MerchantPortal.md`, …) are the index; read source HTML on demand and write findings back into the group file. Groups are identified by filename prefix.
- `agents/PRODUCT_CONTEXT.md` is the product-wide grounding: what Skeps builds, user types (borrower / merchant / lender), the application lifecycle, and the live lender list. Read it before any product discussion in that directory.
- `agents/PRD_AGENT.md` and `agents/DISCUSSION_AGENT.md` are prompt files invoked by name in conversation, following the shape in `PRD_STRUCTURE.md`.
- `SOP.md` refers to `reaser/groups/` — a typo; the directory is `research/groups/`.

## Skills

`Product/.claude/skills/` holds two working skills, scoped to files under `Product/`:

- **`open-issues`** — turns the Notion "Open issues" checklist into a dated leadership status page, plus an optional FNBO-branded HTML mail version (`references/mail-template.html`, `references/logo-*.png`).
- **`rca`** — formats a client-facing RCA from raw incident notes in Notion.

Each skill's `SKILL.md` is **mirrored** into `Product/projects/building agents/<name> agent/SKILL.md`, which also holds a `writeup.md` of design notes. When editing a skill, update both copies and commit both — the mirror is the tracked design record, the `.claude/skills/` copy is what actually loads.

`Product/projects/building agents/best practices for building agents.md` is the standing rubric for agent specs here: trigger → inputs → actions → output → done-condition contracts; named sources for every factual claim; `unknown — needs input from <owner>` over a guess; deterministic retrieval in workflows, judgment in agents. Apply it when writing or reviewing any agent or skill in this repo.

