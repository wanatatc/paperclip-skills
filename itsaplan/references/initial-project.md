# Initial Project

Goal: a project that is ready for the "1 Issue = 1 UI" workflow. Run these steps in order. Each step is **check first, create only what is missing**.

## Ask when

- Project key / name / team are not given (and the project does not exist yet). Use `list_projects` to check; with several teams, `list_teams` and ask which one.
- Whether the team has a BA. If **no**, skip the `[Design]` and `[Spec]` templates and say so in the convention Doc.
- The roles to name in the Spec template (this repo uses `FleetManagement:{Role}`; ask if another project differs).

## 1. Project

If it does not exist: `create_project` (key, name). Then `get_project` and keep the ids.

## 2. Columns (states)

Expect these five, matched by `stateType`:

| Name | stateType |
| --- | --- |
| Backlog | backlog |
| Todo | unstarted |
| In Progress | started |
| Done | completed |
| Canceled | canceled |

Add missing ones with `create_column`; do not rename or reorder existing columns without asking.

## 3. Issue types

Required: `Feature` (UI page), `Task` (subtasks, API-only), `Tech debt` (chores). `Bug` and `Research` are optional. Add missing ones with `create_issue_type`.

## 4. Labels

Required: `Frontend`, `Backend`. Add missing ones with `create_label`.

## 5. Issue templates (7)

Check `list_issue_templates` first. Create missing ones with `create_issue_template`, using `typeId` / `labelIds` resolved from step 3–4. Template bodies are Thai.

| Template name | Type | Label | titleTemplate |
| --- | --- | --- | --- |
| หน้า UI (1 หน้า = 1 งาน) | Feature | — | `ชื่อหน้า/แท็บ (/path)` |
| API อย่างเดียว | Task | — | `API: ชื่อ API` |
| Subtask: Design | Task | — | `[Design] ชื่อหน้า` |
| Subtask: Spec (Handoff) | Task | — | `[Spec] ชื่อหน้า` |
| Subtask: Database | Task | — | `[Database] ชื่อหน้า` |
| Subtask: Frontend | Task | Frontend | `[Frontend] ชื่อหน้า` |
| Subtask: Backend | Task | Backend | `[Backend] ชื่อหน้า/API` |

`descriptionTemplate` content (Thai, markdown):

- **หน้า UI:** Route / ส่วนของหน้า, เป้าหมาย, the five standard subtasks as a numbered list (Design, Spec, Database, Frontend, Backend with owner), "ขอบเขตที่ไม่ทำ".
- **API อย่างเดียว:** Endpoint, ผู้เรียกใช้, เป้าหมาย, the three subtasks (Spec, Database, Backend).
- **Design:** owner BA; deliverable = attachments (Figma link/file, or HTML + CSS + JS files); states which parts are normative vs illustrative; may be split into several Design tasks; checklist Desktop / Mobile / State loading-empty-error / Dialog.
- **Spec (Handoff):** owner BA; **business-level only**. Section "Handoff" with: วัตถุประสงค์/ผู้ใช้, ขั้นตอนการใช้งาน (flow), ข้อมูลที่เกี่ยวข้อง (ชื่อ field ที่ผู้ใช้เห็น/ที่ธุรกิจเรียก, ข้อมูลบังคับหรือไม่, ตัวอย่างค่า), กฎ business, ใครทำอะไรได้ (บทบาท), กรณียกเว้น/ข้อผิดพลาดที่ธุรกิจสนใจ, Acceptance criteria (เขียนเป็นสถานการณ์), คำถามที่ยังค้าง. **Do not** put technical design in it (no endpoint paths, HTTP methods, DTO/JSON shapes, column types, table design, index, status codes, library names) — Dev decides those in `[Database]`/`[Backend]`/`[Frontend]`.
- **Database / Frontend / Backend (identical body):** owner Dev + AI agent; a delivery task list. Body = a note "ตอนเริ่มทำ: ทวน [Spec] → เสนอแผนให้ Dev ตรวจ → บันทึกเป็น checklist", the status block (`## สถานะ` with เสร็จแล้ว / ยังเหลือ / ติดขัด-คำถาม / ตัดสินใจแล้ว / วิธีตรวจ), and an empty `## แผนงาน (รอเสนอตอนเริ่มทำ)`. No pre-filled checklist items. See `dev-delivery.md`.

## 6. Table view

Create with `create_view`, `projectKey`, `name: "Table"`, `icon: "post"`, and exactly this config (copied from the reference view at `/pm-wan/FM/view/1`):

```json
{
  "filters": { "conditions": [] },
  "display": {
    "layout": "table",
    "group": "initiative",
    "subgroup": "none",
    "sort": { "field": "status", "dir": "asc" },
    "properties": ["priority", "status", "startDate", "dueDate", "assignee", "id"],
    "showSubtasks": false,
    "collapseSubtasks": false,
    "separateSubtasks": false,
    "showEmptyGroups": false,
    "showLinks": false,
    "hiddenGroups": [],
    "collapsedGroups": [],
    "pinnedGroup": null,
    "timelineScale": "week",
    "timelineCollapseAll": false,
    "calendarDateField": "dueDate",
    "weekStart": 0
  }
}
```

If a view named "Table" already exists (`list_views`), do not create another; compare its `display` with the above and report differences instead of overwriting.

## 7. Convention Doc

Create the Doc "กติกาการแตกงาน: 1 Issue = 1 งาน (1 UI)" with `create_document` (icon 📐) if `list_documents` shows none. Content = the convention in `SKILL.md` as a Thai page: principles, subtask table for UI and API-only, naming `[ชนิด] ชื่อหน้า/แท็บ (/path)`, order Design → Spec → Database → Backend → Frontend, "details go in checklists", "shared Backend lives on one Issue and is referenced by FM-xx". If an Initiative exists, `link_document_initiative`.

## 8. Initiative (optional)

If the user names a goal/deadline, `create_initiative`. Issues are attached to it with `initiativeId` at creation.

## Done check

Run `get_project`, `list_issue_templates`, `list_views`, `list_documents` once and report to the user what was created vs already present.
