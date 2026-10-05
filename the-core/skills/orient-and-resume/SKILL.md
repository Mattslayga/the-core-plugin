---
name: orient-and-resume
description: Orient in The Core before doing work. Use at the start of a session, when the user says "where were we", "what's next", "pick up", "resume" or "continue", names an existing project or task, or before any capture or update when you have not yet confirmed identity and space this session. Checks who you are acting as and which space to use, then searches and reads existing context before anything new is created.
---

# Orient and resume

The Core holds shared notes, tasks, projects and the links between them for a person and the agents they authorise. Orienting first stops you duplicating work or writing into the wrong space.

These workflows use The Core's hosted MCP tools only. Your host may show them with a prefix (for example `mcp__..._core_search`); the names below are the tool names the server publishes.

## 1. Identity and space

1. Call `core_system` with `view=whoami` to confirm who you are acting as.
2. Call `core_system` with `view=namespaces` to list the spaces you can access.
3. Pick one space for this piece of work and pass its ID, slug or alias as `space` on every call that accepts it.
   - If the user named a space, or only one writable space exists, use it.
   - If several are plausible, ask the user. Never guess a space.
4. If a call is denied or the connector is not signed in, tell the user plainly and stop. Do not retry with a different space to get around a denial.

## 2. What is already in motion

- Call `core_work` with `view=what_next` and the chosen `space` for the current priorities.
- For a named project, find it with `core_search`, then call `core_work` with `view=project_context` and the project `id`.
- `core_work` `view=agenda` shows dated work; `view=followups` lists follow-ups and returns a `cursor` for the next page.
- `core_discover` with `mode=resurface` and the `space` surfaces one open thread worth revisiting.

## 3. Search before you create or answer

- Call `core_search` with a focused `query` in the active space before creating anything or answering from memory. Widen with `spaces` or `all_spaces` only when the user asks for cross-space discovery; never combine the scope options.
- Read results with `core_read`, default `view=orient`. Use `view=headings`, then `view=section` with `section` set to a heading, to read only the part you need. Use `view=full` only when the whole record is needed.
- `view=context` returns a typed record with its links; `view=related` and `view=backlinks` show neighbouring records.

## 4. Continue open work

- Report back briefly: the space, what is in motion (titles with IDs) and the suggested next step. Then continue the work the user wants.
- When you start a task, call `core_update` with `record_type=task`, the task `id` and `action=start`.
- Record progress where it belongs (see the capture-and-link and handoff-and-hygiene skills) instead of creating a new note for every update.

## Truthfulness

- State only what a tool result confirmed. Cite records by title and ID.
- If a search returns nothing, say so; do not imply a record exists.
- Report failed, denied or ambiguous-scope calls plainly.

## Making orientation automatic (optional)

Do not edit the user's global or system instructions. If the user asks how to make this happen every session, offer this text for them to add to their own preferences if they choose:

"When I start work, orient in The Core first: confirm identity and space, check what is next, and search before creating anything. Before you stop, leave a handoff in The Core."
