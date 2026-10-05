---
name: capture-and-link
description: Save knowledge and work to The Core. Use when the user shares a decision, idea, finding, commitment or follow-up worth keeping, says "remember this", "note that", "add a task", "track this" or "start a project", or when work produces context a later agent will need. Searches before creating, captures notes, tasks, projects, people, organisations or opportunities in an explicit space, links related records and reads back what was actually saved.
---

# Capture and link

Use The Core's hosted MCP tools. Confirm identity and space first (see orient-and-resume) if you have not done so this session.

## 1. Decide the record type

`core_capture` can create exactly these types:

- **note**: durable context such as decisions, findings, reference material or meeting outcomes. Requires `title` and `content`; optional `tags`.
- **task**: actionable work. Requires `title`; optional `content`, `priority` (`high`, `normal`, `low`), `project_id`, `followup_for_task_id`, `order_rank`, `tags`, `source`.
- **project**: a container that groups related tasks and notes. Requires `title`; optional `content`, `status` (`planned`, `active`, `blocked`, `completed`), `priority`, `tags`, `source`.
- **person**: a person the user works with. Requires `name` (always pass `name`, not `title`); optional `role_title`, `content`, `tags`, `source`. Any other field is rejected.
- **organisation**: a company or group the user works with. Requires `title`; optional `domain`, `content`, `tags`, `source`. Any other field is rejected.
- **opportunity**: a live deal or prospect. Requires `title`; optional `stage` (`new`, `exploring`, `qualified`, `proposal`, `negotiating`, `won`, `lost`, `deferred`), `content`, `tags`, `source`. Any other field is rejected.
- **feedback**: feedback about The Core itself. Requires `message`; optional `category` (`friction`, `request`, `praise`, `bug`, `general`).

`source` is optional provenance on tasks, projects, people, organisations and opportunities: `import` for items taken from files or exports the user provided, `agent` for items you derived from the conversation. It is set only when a record is created and cannot be changed afterwards through `core_update`. Notes and feedback do not accept `source`.

Every `core_capture` call needs an explicit `space`.

## 2. Search before create

1. Call `core_search` with the key terms in the chosen space.
2. If a matching record exists, read it with `core_read` (`view=orient`) and update it instead of creating a duplicate:
   - Add to it with `core_update` `action=append` and `text`.
   - Add or remove tags with `action=tag` and `add` / `remove`.
   - Rename a note with `action=update_fields` and `fields` set to an object with a new `title`. For people, organisations and opportunities, `update_fields` also changes typed-record fields such as `role_title`, `domain` or `stage`. Note content cannot be replaced through these tools; append a correction instead.
   - Move a task through its lifecycle with `action=start`, `block` (needs `blocked_reason`), `unblock`, `defer` (needs `resurface_at`), `close` or `cancel`.
3. Create a new record only when nothing suitable exists. Give it a specific, searchable title.

## 3. Link related records

- Attach a task to a project at creation with `project_id`.
- For other relationships, call `core_links` with `mode=types` (optionally `from_type` and `to_type`) to see the allowed link shapes, then create one with `core_link` using `source_id`, `target_id` and a listed `rel_type`. Add `reason` when the connection is not obvious.
- People, organisations and opportunities link to notes, tasks, projects and each other only through the shapes `core_links` `mode=types` lists (for example, call it with `from_type=person` before linking a person). Never invent a `rel_type`.
- `core_links` with `mode=list` and `source_id` shows existing links. Links cannot be removed through these tools.

## 4. Read back truthfully

After writing, tell the user what was saved using the tool result: record type, title, ID and space, plus any links created. If a call failed or was denied, say so and show the error; do not report it as saved. Do not claim a record exists, was linked or was updated unless the result confirmed it.

## Good capture habits

- Capture the decision and its reason, not the whole conversation.
- One record per durable idea or actionable piece of work.
- Keep the user's own words for decisions they stated; label your own inferences as inferred.
- Ask before capturing anything sensitive, personal or that the user may not want stored.
