# Create Task

Goal: create one Issue (= one job) plus exactly the subtasks that apply.

## Step 1 — Classify the request

| Request looks like | Parent type | Subtasks to consider |
| --- | --- | --- |
| A page (`/route`) | `Feature` | Design, Spec, Database, Frontend, Backend |
| One tab / dialog of a page with several tabs | `Feature` (1 tab = 1 Issue) | same as above |
| API only (no screen) | `Task` | Spec, Database, Backend |
| Cleanup / chore / not a screen | `Tech debt` | none, or one subtask; use a checklist instead |
| Bug in existing work | `Bug` | none unless it spans layers |

## Step 2 — Decide which subtasks apply

| Subtask | Create when |
| --- | --- |
| `[Design]` | A screen is new or changes visibly **and** the team has a BA |
| `[Spec]` | The team has a BA, or there are open business questions to settle (put them in the Spec) |
| `[Database]` | The Handoff/request says tables, columns, indexes, or seed data change. Not when only existing tables are read |
| `[Frontend]` | The job touches the React app |
| `[Backend]` | The job needs a new or changed endpoint, server rule, or Swagger fix |

Shared work: if two Issues need the same Backend (e.g. edit dialog and new page), put `[Backend]` on **one** Issue and reference it by `FM-xx` in the other. Do not duplicate.

## Ask when

- It is unclear whether this is one page or several tabs/dialogs → ask how to split.
- The request names a page but not its route.
- You cannot tell whether a database change is needed → ask, or create `[Database]` with the question in its description only if the user agrees.
- No BA is stated for this project and the Design/Spec question matters (the user is BA in this repo's team, but confirm on a new project).
- **Start date (`startDate`) — ask this first, before anything else about scheduling.** "วันที่เริ่มงานคือวันไหน?" If the user does not give one (no answer, "ไม่รู้", "วันนี้", or they just want it created), use **today's date** (from the current date in the environment, format `YYYY-MM-DD`). Never leave it empty.
- **Due date (`dueDate`) — ask after the start date.** "วันที่สิ้นสุด/กำหนดส่งคือวันไหน?" If not given, leave it empty (do not invent one). Due date must not be earlier than the start date; if it is, point it out and ask again.
- Priority or initiative are not given and cannot be inferred → ask once for both; default priority `medium`. Initiatives are owned by the PM (`wanatat.c`), so ask the PM's choice only when several are active.

## Step 3 — Resolve ids

`get_project` → `columns` (Backlog = `stateType: backlog`), `issueTypes` (by name), `labels` (`Frontend`, `Backend`). `list_initiatives` if an initiative is needed. Check `list_issue_templates` and use the matching template text as the description skeleton.

Check duplicates: `search_issues` with the page/route name before creating.

## Step 4 — Create the parent

`create_issue`:

- `columnId`: Backlog.
- `typeId`: from Step 1.
- `title` (Thai): UI → `ชื่อหน้า (/route)`; tab → `แท็บชื่อ (/route/:id)`; dialog → `ชื่อ (Dialog ใน /route/:id)`; API → `API: ชื่อ API`.
- `description` (Thai): from the template — Route / ส่วนของหน้า, เป้าหมาย, ขอบเขตที่ไม่ทำ, references to existing Docs.
- `startDate` (always set; user's answer or today), `dueDate` (if given), `initiativeId`, `priority` if known. **No** `parentId`.

## Step 4b — Shape of each subtask kind

- **`[Design]`**: centred on attachments — Figma link/file, or HTML + CSS + JS prototype files (use `add_attachment` or paste links). State which parts are *normative* (layout, fields, states that must match) and which are *illustrative* (mock data, colours). If the BA wants more detail, create several `[Design]` subtasks (e.g. `[Design] ... — ตาราง`, `[Design] ... — ฟอร์ม`); this is the one kind that may repeat per parent.
- **`[Spec]`**: derived from the UI. Skeleton as described below; goal is that a reader can conclude what the UI must do. Business level only.
- **`[Database]` / `[Frontend]` / `[Backend]`**: create with a *placeholder* description (owner + "แผนงานจะเสนอตอนเริ่มทำ — ดู Spec"). Do **not** invent the sub-task checklist at creation time; the plan is proposed when the Dev starts (see `dev-delivery.md`), after reviewing the Spec.

## Step 5 — Create the subtasks

For each applicable subtask, `create_issue` with:

- `parentId`: the parent's numeric `id` (a subtask cannot itself have subtasks).
- `startDate`: same as the parent's start date, unless the user gives per-subtask dates (e.g. Design now, Backend next week). `dueDate`: the user's per-subtask date if given; otherwise the parent's due date if one exists, but never later than the parent's due date. When the user gives dates per subtask, make sure the parent's start is the earliest and its due date the latest; if not, ask whether to widen the parent's dates.
- `typeId`: `Task`.
- `columnId`: Backlog.
- `labelIds`: `[Frontend]` for `[Frontend]`, `[Backend]` for `[Backend]`, none otherwise.
- `title`: `[Design] <parent title>`, `[Spec] …`, `[Database] …`, `[Frontend] …`, `[Backend] …` (same page/tab suffix as the parent).
- `description`: the matching template body, with the known details turned into `- [ ]` checklist items. Put open questions under a "ต้องยืนยัน" heading in `[Spec]`.

Rules:

- Max one subtask per kind per parent, **except `[Design]`** (may repeat when the BA splits the UI). Extra items of other kinds become checklist lines in the existing subtask.
- For `[Database]`/`[Frontend]`/`[Backend]` the description is a placeholder; the checklist is written later by the Dev-approved plan (overrides the "turned into `- [ ]` checklist items" line above for these three kinds).
- Do not pre-fill the Handoff section of `[Spec]` with guesses. Leave fields blank for BA.
- **`[Spec]` is business-level.** Its skeleton asks for: วัตถุประสงค์/ผู้ใช้, ขั้นตอนการใช้งาน, ข้อมูลที่เกี่ยวข้อง (field names as the business calls them, required or not, sample values), กฎ business, ใครทำอะไรได้, กรณียกเว้น, Acceptance criteria as scenarios, คำถามที่ยังค้าง. Field names are welcome; technical design is not (no endpoint paths, HTTP methods, request/response JSON, DTOs, column types, table/index design, status codes). Never put those in a Spec skeleton, and if the user dictates them, keep only the business meaning and tell the user the technical part belongs in `[Backend]`/`[Database]`.
- The "ต้องมี Database หรือไม่" question is **not** a Spec field; decide it with the user at creation time (see "Ask when").
- Do not assign people unless the user says who or confirms the proposed assignee. Proposed assignees come from `SKILL.md` "Team roles", after `list_members` for this project (see `handoff.md`).

## Step 6 — Verify and report

`get_issue` on the parent; confirm the subtask list. Reply with a short table: `FM-xx` identifier, title, kind. Mention anything skipped and why (e.g. "ไม่มี [Database] เพราะใช้ตารางเดิม").

## Examples

- "Add a Fuel Card tab to vehicle detail" → Feature `แท็บบัตรน้ำมัน (/vehicle/:id)` with Design, Spec, Frontend, Backend.
- "New API to search employees" → Task `API: ค้นหาพนักงาน …` with Spec, Backend.
- "Clean up Vehicle module leftovers" → Tech debt with a checklist, no standard subtasks.
