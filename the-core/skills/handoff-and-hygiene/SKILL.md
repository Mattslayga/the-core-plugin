---
name: handoff-and-hygiene
description: Leave The Core in a state a fresh agent can continue from. Use before ending a session or switching tasks, when the user says "wrap up", "hand off", "write a handoff", "save progress" or "I'm done for today", and when the user asks to archive, remove, clean up or restore records. Writes handoffs onto the relevant task or note and describes archive and restore as they behave today.
---

# Handoff and hygiene

Use The Core's hosted MCP tools only. Confirm identity and space first (see orient-and-resume) if you have not done so this session.

## Write a handoff

A handoff lets a fresh agent with no memory of this session continue the work.

1. Find the record the work belongs to with `core_search` and `core_read`: usually the task being worked on, otherwise its project or the main note. Create a new note only if nothing suitable exists.
2. Append the handoff with `core_update` `action=append` and `text`. Include:
   - what was done, with record titles and IDs;
   - decisions made and their reasons;
   - current state, including anything half-finished;
   - open questions and blockers;
   - the single next action, specific enough to start without asking.
3. Update task state to match reality: `action=close` for finished work, `action=block` with `blocked_reason` when stuck, `action=defer` with `resurface_at` (a timestamp in epoch milliseconds) when it should wait.
4. Capture any new follow-up as a task, linked to its project (see capture-and-link).
5. Tell the user where the handoff was written (title and ID). Only report what the tool results confirmed.

Write for a reader with no context: no "as discussed", no unexplained shorthand.

## Archive: request only

Archiving is approval-gated.

- `core_update` with `action=archive`, the `record_type`, the `id` and a short `reason` creates an archive approval request. It does not archive the record.
- The record stays visible and active until a person with approval rights approves the request. Approval review is not available through these tools.
- Tell the user: "I've requested archiving of <title> (<ID>). It stays visible until the request is approved."
- Never describe an archive request as done.

## Restore

`core_update` with `action=restore` clears a record's archived state directly, with no approval step. Read the record back with `core_read` and confirm the result to the user.

## What these tools cannot do

Hard deletion, rewriting note content, removing links and reviewing approvals are not available through these tools. Say so plainly if asked, and suggest the nearest supported step: append a correction, cancel a task or request archiving.

## Hygiene check (optional)

When the user asks to tidy a space, call `core_discover` with `mode=stewardship_summary` and the `space`, then `mode=stewardship_next` for one concrete item. `mode=orphans` and `mode=stale` find unlinked or neglected notes. Propose changes and act only on what the user approves.
