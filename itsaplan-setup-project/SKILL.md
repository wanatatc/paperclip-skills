---
name: itsaplan-setup-project
description: Set up a new Itsaplan project for "1 Issue = 1 UI" (columns, types, labels, 7 templates, Table view, convention Doc, optional initiative). Use for "initial project", "ตั้งค่าโปรเจกต์", "สร้างโปรเจกต์ใหม่". Read AGENT.md first.
---

# Itsaplan — Setup Project

Run in order; each step: check first, create only what's missing. Shared rules: `AGENT.md`.

## Ask when
- Key/name/team missing (`list_projects`; several teams → `list_teams`).
- Team has a BA? If no, skip Design/Spec templates and note it in the Doc.
- Role names for Spec template (default `FleetManagement:{Role}`).

## 1. Project
`create_project` if absent, then `get_project`.

## 2. Columns (by `stateType`)
Backlog=backlog, Todo=unstarted, In Progress=started, Done=completed, Canceled=canceled. `create_column` for missing; don't rename/reorder without asking.

## 3. Issue types
Required `Feature`, `Task`, `Tech debt`; optional `Bug`, `Research` (`create_issue_type`).

## 4. Labels
`Frontend`, `Backend` (`create_label`).

## 5. Templates (7; `list_issue_templates` first; Thai bodies)
| Name | Type | Label | titleTemplate |
| --- | --- | --- | --- |
| หน้า UI (1 หน้า = 1 งาน) | Feature | — | `ชื่อหน้า/แท็บ (/path)` |
| API อย่างเดียว | Task | — | `API: ชื่อ API` |
| Subtask: Design | Task | — | `[Design] ชื่อหน้า` |
| Subtask: Spec (Handoff) | Task | — | `[Spec] ชื่อหน้า` |
| Subtask: Database | Task | — | `[Database] ชื่อหน้า` |
| Subtask: Frontend | Task | Frontend | `[Frontend] ชื่อหน้า` |
| Subtask: Backend | Task | Backend | `[Backend] ชื่อหน้า/API` |

Bodies:
- **หน้า UI:** Route/ส่วนของหน้า, เป้าหมาย, 5 subtasks numbered with owner, ขอบเขตที่ไม่ทำ.
- **API อย่างเดียว:** Endpoint, ผู้เรียกใช้, เป้าหมาย, 3 subtasks (Spec, Database, Backend).
- **Design:** owner BA; deliverable = attachments (Figma or HTML+CSS+JS); normative vs illustrative; may split; checklist Desktop / Mobile / State loading-empty-error / Dialog.
- **Spec:** owner BA; business only. Section "Handoff": วัตถุประสงค์/ผู้ใช้, ขั้นตอนการใช้งาน, ข้อมูลที่เกี่ยวข้อง (ชื่อ field ธุรกิจ, บังคับไหม, ตัวอย่างค่า), กฎ business, ใครทำอะไรได้, กรณียกเว้น, Acceptance criteria (สถานการณ์), คำถามที่ยังค้าง. No technical design (endpoints, methods, DTO/JSON, column types, tables, indexes, status codes, libraries).
- **Database/Frontend/Backend (same body):** owner Dev + AI; note "ตอนเริ่มทำ: ทวน [Spec] → เสนอแผนให้ Dev ตรวจ → บันทึกเป็น checklist", status block (`## สถานะ`: เสร็จแล้ว / ยังเหลือ / ติดขัด-คำถาม / ตัดสินใจแล้ว / วิธีตรวจ), empty `## แผนงาน (รอเสนอตอนเริ่มทำ)`. No pre-filled items.

## 6. Table view
`list_views` first; if "Table" exists, report differences, don't overwrite. Else `create_view` (`name: "Table"`, `icon: "post"`):
```json
{"filters":{"conditions":[]},"display":{"layout":"table","group":"initiative","subgroup":"none","sort":{"field":"status","dir":"asc"},"properties":["priority","status","startDate","dueDate","assignee","id"],"showSubtasks":false,"collapseSubtasks":false,"separateSubtasks":false,"showEmptyGroups":false,"showLinks":false,"hiddenGroups":[],"collapsedGroups":[],"pinnedGroup":null,"timelineScale":"week","timelineCollapseAll":false,"calendarDateField":"dueDate","weekStart":0}}
```

## 7. Convention Doc
If `list_documents` has none: `create_document` "กติกาการแตกงาน: 1 Issue = 1 งาน (1 UI)" (icon 📐), Thai: principles, subtask table (UI / API-only), naming `[ชนิด] ชื่อหน้า/แท็บ (/path)`, order Design → Spec → Database → Backend → Frontend, details in checklists, shared Backend on one Issue referenced by `FM-xx`. Link with `link_document_initiative` if an Initiative exists.

## 8. Initiative (optional)
User names goal/deadline → `create_initiative` (owner `wanatat.c`); Issues attach via `initiativeId`.

## Done
Run `get_project`, `list_issue_templates`, `list_views`, `list_documents`; report created vs existing.
