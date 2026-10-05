---
name: initialise-context
description: Build a person's first context in The Core from material they provide. Use when the user says "set me up", "get me started", "initialise", "import my context", "bring in my notes" or "here is my memory export", or shares files or an export to load into a new or nearly empty space. Works only from the current conversation, files the user uploaded and exports the user provided. Proposes one previewed batch and creates nothing until the user approves it.
---

# Initialise context

Turn what the user has already given you into a small, well-linked starting set of notes, projects, tasks, people and organisations (opportunities too, when the user describes a live deal or prospect). Use The Core's hosted MCP tools only.

## Allowed sources

Use only:

- the current conversation;
- files the user uploaded in this conversation;
- memory or history exports the user explicitly provided.

Do not read other accounts, connectors, mailboxes, drives or the web to gather material. If something important is missing, ask the user.

## 1. Orient

Confirm identity and space with `core_system` (`view=whoami`, then `view=namespaces`). Ask which writable space should receive the context if more than one is plausible. Search what the space already holds with `core_search` so you do not duplicate it.

## 2. Draft one batch

Extract the durable items: active projects, open tasks, key decisions, preferences, working context and reference facts. Keep it small: a useful starting set beats an exhaustive dump. The creatable types are note, task, project, person, organisation and opportunity. People and organisations the user works with become their own person or organisation records, not details folded into a note. Create opportunities only when the user describes a live deal or prospect.

For every proposed item record:

- type and title;
- one-line summary of the content;
- source label, for example "conversation", "upload: plan.pdf" or "export: assistant-memory.json";
- provenance value for a new record: `source=import` for items from uploaded files or exports, `source=agent` for items you derived from the conversation. `source` can only be set at creation; for an existing matched record, propose an appended provenance line instead. Notes cannot carry `source`, so their provenance lives in their content;
- **confirmed** (the user stated or explicitly agreed it) or **inferred** (you derived it);
- whether it matches an existing record, in which case propose an update rather than a new record;
- proposed links, for example task to project.

## 3. Preview and get explicit approval

Show the whole batch once, before writing anything:

- counts per type (for example: 2 projects, 3 people, 1 organisation, 5 tasks, 4 notes, 3 updates to existing records);
- counts of confirmed versus inferred items;
- the item list grouped by type with source labels;
- the destination space;
- this limitation, stated plainly: "The Core does not yet keep an import receipt or offer whole-batch undo. If something is wrong after import, it has to be corrected record by record."

Ask the user to approve, edit or drop items. Create nothing until they explicitly approve. If they change the batch, show the revised counts and confirm again.

## 4. Create and link

After approval:

1. Search again for each item title, then create it with `core_capture` (explicit `space`) or update the matching record with `core_update`.
2. Create projects, organisations and people first, so tasks can use `project_id` and links can reference them. Use `core_links` `mode=types` and `core_link` for other relationships, for example linking a person to their organisation.
3. Set `source` on each new typed record in the `core_capture` call that creates it (`import` for items from uploaded files or exports, `agent` for items you derived). `source` cannot be set on existing records; when updating a matched record, append the provenance label (`source`, confirmed or inferred) to its content with `core_update` `action=append` instead. Put the provenance label and confirmed or inferred in each new record's content too; content is how notes carry provenance, since notes do not accept `source`.
4. Tag every created or updated record with one shared batch tag, for example `setup-2026-10-01`, so the batch can be found together later with `core_search`.

## 5. Report and explain corrections

Report what was created and updated (type, title, ID), what failed and why, and the batch tag. Explain how to correct mistakes today:

- rename a note with `core_update` `action=update_fields` and a new `title`;
- correct typed fields on people, organisations and opportunities, such as `role_title`, `domain` or `stage`, with `action=update_fields`;
- add a correction with `action=append` (note content cannot be replaced through these tools);
- fix tags with `action=tag`;
- cancel a wrong task with `action=cancel`;
- request removal with `action=archive`, which only creates an approval request; the record stays visible until a person with approval rights approves it.
