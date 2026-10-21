---
name: itsaplan
description: Use Itsaplan (the project tracker behind project.wanatat.cloud, MCP tools for issues/views/templates/docs) with this team's "1 Issue = 1 UI" convention. Covers setting up a new project (columns, labels, templates, Table view), creating a task with its standard subtasks (Design/Spec/Database/Frontend/Backend), and handing work between BA and Dev. Trigger on "Itsaplan", "สร้าง task", "สร้างงาน", "ส่งต่องาน", "handoff", "initial project", or any request to create/move/update issues in the FM project.
---

# Itsaplan — Project Workflow

This skill is a router. Read **only** the reference file that matches the request, then follow it.

| User wants to... | Read |
| --- | --- |
| Set up a new project (columns, labels, templates, Table view, convention doc) | `references/initial-project.md` |
| Create a task / page / API (Issue + subtasks) | `references/create-task.md` |
| Pass work on (finish a subtask, hand BA → Dev, close a parent) | `references/handoff.md` |
| Dev starts or resumes `[Database]`/`[Frontend]`/`[Backend]` (review Spec, propose plan, keep status) | `references/dev-delivery.md` |

If a request spans several topics (e.g. "create the task, then hand off Design"), read each file in order.

## Language

- Instructions here are English.
- Everything written **into Itsaplan** (titles, descriptions, comments, checklists) is **Thai**, matching the existing issues.

## The convention (one paragraph)

**1 Issue = 1 job = 1 UI page** (a page with several tabs is split: 1 tab = 1 Issue). Each UI Issue gets at most five subtasks: `[Design]`, `[Spec]`, `[Database]`, `[Frontend]`, `[Backend]`. An API-only job is one Issue with `[Spec]`, `[Database]`, `[Backend]`. Fine-grained items are **checklists inside a subtask's description**, never extra subtasks. Skip subtasks that do not apply (e.g. no `[Database]` when no schema change; no `[Spec]`/`[Design]` when the team has no BA). Roles of the subtasks:
- `[Design]` is about **attachments**: a Figma link/file or an HTML + CSS + JS prototype. One UI may be split into several `[Design]` tasks when the BA wants more detail.
- `[Spec]` summarises, from the UI, what the screen must do in business terms — enough that someone can conclude how the UI must behave.
- `[Database]`/`[Frontend]`/`[Backend]` are **delivery task lists**: on start the agent reviews the Spec with the Dev and proposes a plan of sub-tasks for the Dev to check; afterwards the tracker records done / missing / key decisions, not how the code was written (see `references/dev-delivery.md`).

`[Spec]` is **business-level**: purpose, flow, field names, business rules, who can do what, edge cases, acceptance scenarios. No technical design (endpoints, DTOs, column types, table design) — Dev owns that. The same rules live in the project Doc "กติกาการแตกงาน: 1 Issue = 1 งาน (1 UI)".

## Rules that apply to every topic

1. **Resolve ids first.** Call `get_project` with the project key (e.g. `FM`) and take column, type, label, and member ids from it. Never invent or reuse ids across projects.
2. **Select columns by `stateType`** (`backlog`, `unstarted`, `started`, `completed`, `canceled`), never by column name.
3. **Select issue types and labels by name** from `get_project` (`Feature`, `Task`, `Tech debt`, `Frontend`, `Backend`), then use the returned id.
4. **Look before you write.** Use `list_issues` / `search_issues` / `list_issue_templates` / `list_views` to check what already exists. Do not create duplicates.
5. **Identifiers:** a person writes `FM-12` → `get_issue_by_number` with projectKey `FM`, sequenceNumber `12`. Most other tools take the numeric internal `id` from that result.
6. **Ask the user** when something is ambiguous (see each reference's "Ask when" list). Do not guess page boundaries, who is BA, or whether a database change is needed.
7. **Deleting is not allowed** unless the user asks. Use `archive_issue` (restorable) when merging or removing. Before archiving a subtask, copy its description into the subtask that replaces it.
8. **Dates:** every task has a start date and may have a due date. Always ask for the start date first; if the user does not give one, use today's date. Ask for the due date next; if not given, leave it empty. Format `YYYY-MM-DD`. Details in `references/create-task.md`.
9. After any batch of writes, **verify** with `get_issue` on the parent and report the resulting identifiers (`FM-xx`) to the user.
