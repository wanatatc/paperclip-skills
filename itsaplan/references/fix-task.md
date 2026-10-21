# Fix Task

Goal: BA records a list of defects to fix in one parent Issue `[FIX]`, then hands it to the Dev as delivery subtasks. Use this for fixing existing work, not for new features.

## Step 1 — Gather the fix list from the BA

Ask the BA, one question at a time, until every item is clear:

1. **แก้อะไรบ้าง?** Ask for the list of problems (each becomes one numbered item).
2. For **each** item, ask for:
   - หน้า/route ที่เกิดปัญหา
   - ตอนนี้เป็นอย่างไร (อาการที่เห็น)
   - ควรเป็นอย่างไร (ผลที่ถูกต้อง)
   - ตัวอย่างข้อมูล/ขั้นตอนที่ทำให้เกิด (ถ้ามี)
3. **ต้องแก้ฐานข้อมูลหรือ backend หรือไม่?** Ask per item. Items that only change how the UI looks or behaves are frontend-only.

If the BA gives a vague item (e.g. "แก้ UI"), do not guess. Ask what is wrong on that screen and what it should show.

## Step 2 — Decide which subtasks apply

| Subtask | Create when |
| --- | --- |
| `[Database]` | At least one item changes tables, columns, indexes, or seed data |
| `[Backend]` | At least one item needs an endpoint, server rule, or data fix |
| `[Frontend]` | At least one item changes the React app |

If only the frontend changes, create only `[Frontend]`. No `[Design]` or `[Spec]` for FIX: the parent list is the spec.

## Step 3 — Resolve ids

`get_project` → `columns` (Backlog = `stateType: backlog`), `issueTypes` (`Bug`, `Task`), `labels` (`Frontend`, `Backend`). Check `search_issues` for an existing `[FIX]` on the same page before creating.

## Step 4 — Create the parent

`create_issue`:

- `columnId`: Backlog.
- `typeId`: `Bug`.
- `title`: `[FIX] ชื่อหน้า/งาน (/route)`.
- `description` (Thai), the list the BA gave:

```
## รายการที่ต้องแก้
1. <สิ่งที่ต้องแก้>
   - หน้า: /route
   - ตอนนี้: ...
   - ควรเป็น: ...
   - ฐานข้อมูล/backend: ใช่ / ไม่ใช่
2. <สิ่งที่ต้องแก้>
   - ...
```

- `startDate` (always set, see Step 6), `dueDate` (if given), `priority` if known.

## Step 5 — Create the subtasks

For each applicable subtask, `create_issue` with:

- `parentId`: the parent's numeric `id`.
- `typeId`: `Task`.
- `columnId`: Backlog.
- `labelIds`: `[Frontend]` or `[Backend]` for those kinds; none for `[Database]`.
- `title`: `[Database] [FIX] ชื่อหน้า/งาน`, `[Frontend] [FIX] ชื่อหน้า/งาน`, `[Backend] [FIX] ชื่อหน้า/งาน`.
- `startDate`/`dueDate`: same as parent, unless the user gives different dates.
- `description`: placeholder only, like other delivery subtasks: owner + "แผนงานจะเสนอตอนเริ่มทำ — ดูรายการที่ต้องแก้ใน [FIX] หลัก". The Dev writes the checklist when starting (see `dev-delivery.md`).

## Step 6 — Dates

Same rules as `create-task.md`: ask the start date first (today if not given), then the due date (empty if not given). Due date must not be earlier than the start date.

## Step 7 — Verify and report

`get_issue` on the parent; confirm the subtask list. Reply with a short table: `FM-xx` identifier, title, kind, and which subtasks were skipped and why.

## Handoff to Dev

Once created, the BA hands off per `handoff.md` (FIX flow): open the first existing subtask in order `[Database]` → `[Backend]` → `[Frontend]` in `unstarted` (Todo). Ask who the Dev is first if the project has more than one candidate.

## Examples

- "หน้า /employee มี 2 bug: ปุ่มบันทึกไม่ทำงาน, ตารางแสดงวันที่ผิด" → Parent `[FIX] พนักงาน (/employee)` with 2 items; subtasks `[Frontend]` (+ `[Backend]` if the save endpoint is wrong).
- "แก้ตารางข้อมูลซ้ำใน DB และ API ส่งค่าผิด" → Parent with `[Database]`, `[Backend]`.
