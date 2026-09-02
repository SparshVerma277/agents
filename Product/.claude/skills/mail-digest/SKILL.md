---
name: mail-digest
description: Summarise mail threads into action items by owner and publish them as a dated subpage under the Daily mail page in Notion, newest at the top. Asks which mode and which mail to include on each run.
---

Replaces the earlier unread-digest skill. Same workflow, with a selectable input mode and enforced newest-first ordering in Notion.

## When to use

When the user asks for their mail digest, a summary of busy or recent threads, or invokes /mail-digest. Typically run once a day, first thing.

## Input: ask before you run

The user picks the mode each time. If the skill was invoked with no arguments, ask before querying anything. Use `ask_user_question` with two questions in one call:

1. **Mode** — options: `Unread threads (3+)`, `Everything since yesterday`, `Specific sender or partner`, `One thread`.
2. **Scope** — options: `All owners`, `Only items on me`, `Only external (FNBO / EXL)`, `Only internal (Skeps)`.

If arguments were supplied, parse them and skip the question. Supported shapes:

- `/mail-digest` — ask.
- `/mail-digest unread` — threads with 3 or more unread. **"More than 3" means 3 or more** (`>= 3`). This was an explicit user correction; do not revert to a literal reading.
- `/mail-digest unread 5` — same with a different threshold.
- `/mail-digest since yesterday` / `since 2026-09-01` / `today` — every inbox thread with a message received in that window, read or not.
- `/mail-digest from fnbo.com` / `from Cody` / `from anahita` — threads where that person or domain sent a message. Match on sender address, not body text.
- `/mail-digest thread "UCL Rate Corrections"` — one named thread, full history.
- Append `mine` to any of the above to keep only threads with an open item on Sparsh.

Echo the resolved mode back in one line before running, e.g. "Unread threads, 3 or more, all owners." Done when: mode and scope are fixed and stated.

## Constants that do not change

- **Inbox folder only.** Not Sent Items, not Archive, even though Outlook's conversation view counts across folders.
- **Group by conversation id.** Subject grouping gives the same answer in practice but conversation id is the source of truth.
- Owners the user tracks: Sparsh (the user), Anahita Chaudhary, Rahul Gupta, Rachit Singhal, Manish Jain, Ishan Diwakar. Nidhi Gupta, Sheel and Mandeep often own internal ops items and are worth naming.

## Step 1 — Select the threads

One `execute_office_js` call against `/me/mailFolders/inbox/messages`, selecting `id,subject,conversationId,isRead,from,receivedDateTime`. Apply the mode's filter, group by `conversationId`.

Report the qualifying thread count to the user before going further, so a wildly wrong number is visible early.

Done when: you have the qualifying conversation ids with counts.

## Step 2 — Read each thread properly

Fetch **all** messages in each qualifying conversation, not only the unread ones. Unread-only reads systematically understate long threads: on a 3-of-12 thread the unread tail is often "are you back?" and "thanks" while the decisions sit in already-read messages.

Raw `body.content` re-quotes the whole chain in every reply, so 100 messages can exceed a million tokens. Strip it:

```js
(m.body?.content || "")
  .replace(/<[^>]+>/g, " ")
  .replace(/&nbsp;/g, " ")
  .replace(/\s+/g, " ")
  .split(/From: |________/)[0]
  .slice(0, 900)
```

Cutting at the first `From:` or `____` drops the quoted trail, where nearly all the redundancy lives. Stash bulk results with `blobs.setJSON` and return only what you need.

Done when: every thread is in chronological order with quoted trails stripped.

## Step 3 — Summarise

Per thread:

- Two to four sentences of narrative: what was raised, what was found, where it landed. Name the people.
- A bold **Action:** line with the owner and the concrete next step.
- Check whether a later message already closed the ask before calling it open.
- Flag asks nobody answered, including the user's own unanswered questions to his team. These are the highest-value items and are easy to miss because they sit in read messages.
- Keep ticket numbers, loan and application ids, and dates verbatim. This user works in them.
- Note any gateway flags (`[SUSPICIOUS MESSAGE]`, hidden-CSS warnings).

## Step 4 — Publish to Notion, newest at the top

Always a subpage under the **Daily mail** page, id `3cf9bcf9-cd5d-80dd-bdaf-fd634119974a`. Confirm the parent still exists with a search before writing.

Page shape:
- Title: today's date as `YYYY-MM-DD`. Icon: 📬. If a page for today already exists, title the new one `YYYY-MM-DD (2)` rather than overwriting.
- First line states the mode used, e.g. "Threads from FNBO since 1 September. Times in IST."
- H2 sections in order: **Action on me (Sparsh)**, **Action on the team**, **No action on us**. Omit a section that would be empty.
- Each thread an H3 with subject and count, e.g. `### UCL Rate Corrections — 3 unread`.
- British spelling (en-IN), no em dashes in body prose.

**Ordering — today's subpage must appear first.** `notion-create-pages` appends the child page block at the bottom, so fix the order after creating:

1. Fetch the Daily mail page to get its current content, including the `<page url="...">` child tags in their existing order.
2. Call `notion-update-page` with `command: "update_content"`, using an `old_str` containing the previously-newest tag followed by the tag you just created, and a `new_str` with those two swapped. Both tags remain present, so this is a reorder and not a deletion; do not pass `allow_deleting_content`.
3. Fetch once more and confirm today's page is the first child.

If the reorder fails, do not retry blind. Fall back to `command: "insert_content"` with `position: {"type": "start"}`, adding one line linking to today's page, and tell the user the child block itself is still at the bottom.

Done when: the page exists and today's entry is the first item under Daily mail.

## Step 5 — Report back in chat

Give the URL, then the per-thread summaries inline with `` chips so the user can jump straight to each thread. Do not make him open Notion to see what you found.

## Notes

- Start a new chat for each run. Fresh context means a sharper read.
- If a mailbox read is declined, say plainly that it was not performed and do not retry without instruction.
