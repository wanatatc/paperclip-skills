---
name: itsaplan-create-task
description: Create one Itsaplan Issue (UI page, tab, dialog, or API) plus applicable subtasks ([Design], [Spec], [Database], [Frontend], [Backend]). Use for "สร้าง task", "สร้างงาน", new page/API task.
---

# Itsaplan — Create Task

Convention: **1 Issue = 1 job = 1 UI page** (multi-tab page → 1 tab = 1 Issue). Details go in checklists, not extra subtasks.

## Rules
- Resolve ids with `get_project` (key e.g. `FM`); never invent or reuse ids across projects. Pick columns by `stateType`, types/labels by name.
- Check duplicates before creating. `FM-12` → `get_issue_by_number`.
- All text written to Itsaplan (title, description, comment, checklist) in Thai. Ask when unclear; never delete (use `archive_issue`); no secrets/real customer data.
- Dates `YYYY-MM-DD`.

## Team & assignees
| Username | Role | Owns |
| --- | --- | --- |
| `wanatat.c` | PM | Initiatives |
| `savepop48566` | BA | `[Design]`, `[Spec]` |
| `wuthichai.chuichaeng` | Dev DB/Backend | `[Database]`, `[Backend]` |
| `jomchai.suk09` | Dev Frontend | `[Frontend]` |

Call `list_members` first: only `wanatat.c` → all roles to them; 2 members with `wanatat.c` owner → all to the other; else use the table (role person not a member → fall back to the above or ask). Ask "จะใช้คนตามนี้เลยไหม?" with the proposal per subtask; assign only after confirmation, userIds only from `list_members`.

## 1. Classify

| Request | Parent type | Subtasks |
| --- | --- | --- |
| Page (`/route`) or one tab/dialog | `Feature` | Design, Spec, Database, Frontend, Backend |
| API only | `Task` | Spec, Database, Backend |
| Chore | `Tech debt` | none (use checklist) |
| Bug | `Bug` | none; fix list → `itsaplan-fix-task` |

## 2. Which subtasks

- `[Design]`: screen new/visibly changed **and** team has BA
- `[Spec]`: team has BA, or open business questions exist
- `[Database]`: tables/columns/indexes/seed change (not read-only)
- `[Frontend]`: touches React app
- `[Backend]`: new/changed endpoint, server rule, Swagger fix

Shared Backend across Issues: put `[Backend]` on one Issue, reference by `FM-xx` in the other.

## Ask when
- Unclear one page vs several tabs; route missing; DB change unclear; BA unknown.
- Start date first (none → today), then due date (none → empty; not before start).
- Priority/initiative unknown → ask once; default priority `medium`; ask initiative only if several active.
- Assignees: confirm before assigning (see Team & assignees).

## 3. Resolve
`get_project` (Backlog column, types, labels `Frontend`/`Backend`), `list_initiatives` if needed, `list_issue_templates` (use as description skeleton), `search_issues` for duplicates.

## 4. Parent (`create_issue`)
Backlog; type from step 1; no `parentId`; `startDate` always set; `dueDate`, `initiativeId`, `priority` if known.
Title: UI `ชื่อหน้า (/route)`; tab `แท็บชื่อ (/route/:id)`; dialog `ชื่อ (Dialog ใน /route/:id)`; API `API: ชื่อ API`.
Description (template): Route/ส่วนของหน้า, เป้าหมาย, ขอบเขตที่ไม่ทำ, Doc references.

## 5. Subtasks (`create_issue`)
`parentId` = parent numeric id; type `Task`; Backlog; labels `Frontend`/`Backend` only on those; title `[Kind] <parent title>`; `startDate` = parent's; `dueDate` = per-subtask if given, else parent's, never later than parent's (if per-subtask dates fall outside parent's range, ask to widen parent).

Per kind:
- **Design**: attachments (Figma, or HTML/CSS/JS via `add_attachment`/links); mark *normative* (layout, fields, states) vs *illustrative* (mock data, colours). May repeat per parent (`[Design] ... — ตาราง`).
- **Spec**: วัตถุประสงค์/ผู้ใช้, flow, ข้อมูลที่เกี่ยวข้อง, กฎ business, ใครทำอะไรได้, กรณียกเว้น, Acceptance criteria (สถานการณ์), คำถามที่ยังค้าง/ต้องยืนยัน. Business only — no endpoints, HTTP methods, JSON/DTO, column types, table/index, status codes; if user gives these, keep business meaning and say they belong in Backend/Database. Leave Handoff fields blank; don't guess. "ต้องมี Database ไหม" is not a Spec field.
- **Database/Frontend/Backend**: placeholder only ("แผนงานจะเสนอตอนเริ่มทำ — ดู Spec"); no checklist (see `itsaplan-dev-delivery`).

Max one subtask per kind (except Design); extras become checklist lines.

## 6. Verify
`get_issue` parent; report table (`FM-xx`, title, kind, assignee) + skipped subtasks with reason (e.g. "ไม่มี [Database] เพราะใช้ตารางเดิม").
