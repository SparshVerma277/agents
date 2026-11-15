# mail-digest — design record

## Where the canonical copy lives (this one inverts the usual rule)

Every other skill in this repo is canonical at `Product/.claude/skills/<name>/SKILL.md`, mirrored here. **mail-digest is the opposite.** It is not a Claude Code skill at all:

- **Canonical:** uploaded to the Claude account (Settings → Capabilities → Skills). That upload is what actually executes.
- **Rendered source of truth on paper:** Notion page 📪 `/mail-digest`, id `3cf9bcf9-cd5d-8056-955e-ee6f8ef0fca9`, under Communications / Daily mail.
- **This file's sibling `SKILL.md`:** a local copy transcribed from that Notion page on **2026-09-02**.
- **`Product/.claude/skills/mail-digest/SKILL.md`:** byte-identical to the sibling, added 2026-09-02 at the user's explicit instruction so the repo keeps its usual two-copy shape.

Editing either local copy changes nothing about what runs. To change behaviour, edit the Notion page, re-upload the ZIP to claude.ai, then re-transcribe both files.

**Known hazard in the `.claude/skills/` copy.** Claude Code will load and can auto-invoke it, but it cannot execute: the skill calls `execute_office_js` against Microsoft Graph, which exists only in the Outlook add-in runtime. A Claude Code run will get as far as the mode question and then fail at Step 1. This was raised before the file was added and the user chose to add it anyway; if it becomes a nuisance, delete `Product/.claude/skills/mail-digest/` — nothing else depends on it.

## Why it runs in Outlook and the Claude Code digest does not

The two paths authenticate as different Entra applications:

| Path | Entra app | Status on the Skeps tenant |
|---|---|---|
| Claude for Outlook add-in | `c2995f31-11e7-4882-b7a7-ef9def0a0266` | Working — Graph token stays in the browser |
| `claude.ai Microsoft 365` MCP connector | `07c030f6-5743-41b7-ba00-0a6e85f37c17` | Blocked by Conditional Access, `AADSTS53003`, re-confirmed 2026-09-02 |

So the digest works in the Outlook pane and cannot work from a Claude Code session. The `morning-mail-digest` memory describes the manual-paste fallback for that case; this skill supersedes it wherever the add-in is available.

Skills enabled in Claude settings are applied automatically inside the Excel / PowerPoint / Word / Outlook add-ins, matched on the `description` field — there is no slash-command dispatch in the pane. That is why the description names the trigger explicitly and why the argument grammar is also documented on the 💡 *How to use claude skill* page (`3cf9bcf9-cd5d-80c8-82f6-f46d3909ee54`) for the user's own reference.

## Decisions worth keeping

- **`unread` means `>= 3`, not `> 3`.** Recorded in the skill as an explicit user correction because a literal reading of "more than 3" regresses it.
- **Read whole threads, not just unread messages.** The unread tail of a long thread is usually "are you back?" and "thanks"; the decisions sit in messages already read. Unread-only reads systematically understate.
- **Strip quoted trails before summarising.** Every reply re-quotes the chain, so raw `body.content` on 100 messages can exceed a million tokens. Cut at the first `From:` or `____` and cap at 900 characters.
- **Inbox folder only, grouped by `conversationId`.** Outlook's conversation view counts across folders, which inflates thread counts.
- **Newest-first is enforced after the write,** because `notion-create-pages` appends the child block at the bottom. The reorder is a tag swap via `update_content`, never a delete.
- **Report the qualifying thread count before reading anything,** so a wildly wrong filter is visible before the expensive step.

## Known gaps

- The transcription has one lossy spot: Step 5 reads "summaries inline with `` chips", where the Notion source carries an empty inline-code span. Whatever chip type was intended did not survive the export. Resolve against the uploaded skill before relying on that line.
- There is no automated check that this copy matches the uploaded skill. The `diff` discipline used for `open-issues` and `rca` cannot apply — the other side is not a file.
- Publishing writes to Notion, unlike the two shipped Claude Code skills, which stop at a draft. The boundary that is preserved is the mail one: the add-in never sends, and every draft lands unsent.
