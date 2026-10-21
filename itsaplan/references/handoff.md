# Handoff (ส่งต่องาน)

Goal: when one subtask is finished, close it properly and pass the work to the next role. Handoff = **move column + change assignee + summary comment**.

## Order of work

`[Design]` (BA) → `[Spec]` (BA) → `[Database]` (Dev, if present) → `[Backend]` (Dev) → `[Frontend]` (Dev)

For a `[FIX]` parent (see `fix-task.md`) there is no Design or Spec. The BA's fix list in the parent is the handoff: the BA opens the first existing subtask in order `[Database]` → `[Backend]` → `[Frontend]` directly, with the summary comment pointing to the `[FIX]` parent.

- `[Database]` and `[Backend]` come before `[Frontend]` when the Frontend needs a new endpoint. Frontend may start early only against a Swagger contract the user confirms.
- Skip steps whose subtask does not exist; the next existing subtask is the receiver.
- Roles: `[Design]`/`[Spec]` = BA (`savepop48566`). `[Database]`/`[Backend]` = Dev DB/Backend (`wuthichai.chuichaeng`). `[Frontend]` = Dev Frontend (`jomchai.suk09`). See the team roles table in `SKILL.md`.

## Ask when

- **Who is the receiver?** Follow the "Team roles" section in `SKILL.md`: check `list_members` for the project first (solo project → `wanatat.c`; owner plus one other → that other person; otherwise the role table). Propose the receiver and ask "จะใช้คนตามนี้เลยไหม?" before assigning. Ask separately only if the role maps to no project member or the user wants someone else.
- A sender subtask has unchecked checklist items or blank Handoff fields and the user asks to pass it on → ask whether to proceed anyway.
- The deliverable (Design HTML, Handoff text) is missing or its location is unclear.

## Procedure: finish a subtask and hand off

1. **Find the subtask.** `get_issue_by_number` (e.g. `FM-37`) or `get_issue` on the parent to see its `subtasks`.
2. **Check the deliverable is really there:**
   - `[Design]`: an attachment (`list_attachments`) or a link/HTML in the description or a comment.
   - `[Spec]`: the business-level Handoff is filled — วัตถุประสงค์, flow, ข้อมูลที่เกี่ยวข้อง (field names), กฎ business, ใครทำอะไรได้, กรณียกเว้น, acceptance criteria — and "คำถามที่ยังค้าง" has no open items. Do **not** require or add technical content (endpoints, DTOs, column types); that is the Dev's design work. If the Spec contains technical detail, leave it, but do not copy it into the summary comment as a requirement.
   - `[Database]`/`[Backend]`/`[Frontend]`: checklist items are ticked; for Backend the Swagger matches the real response.
   If something is missing, stop and tell the user; do not close it.
3. **Close the sender:** `update_issue` with `columnId` = the column whose `stateType` is `completed`.
4. **Write the summary comment** on the sender with `add_comment` (Thai):
   ```
   ส่งต่อ: <ชนิดงาน> → <ชนิดงานถัดไป> (FM-xx)
   สิ่งที่ส่งมอบ: <ลิงก์/ไฟล์/สรุปสั้นๆ>
   ที่ Dev/ผู้รับต้องรู้: <ข้อตกลง, ข้อจำกัด, สิ่งที่ยังไม่ทำ>
   คำถามที่ยังค้าง: <ถ้ามี>
   ```
   Mention the receiver with `@username` only if the user confirmed who. Post the same pointer as a short comment on the **receiver** subtask so it is self-contained.
5. **Open the receiver:** `update_issue` on the next subtask with `columnId` = `unstarted` (Todo), and `assigneeUserId` if the user named one.
6. **Parent state:** if the parent is still in `backlog`, move it to `started` when the first subtask is moved to `started` (not when it is only moved to Todo).
7. **Report** to the user: sender closed, receiver opened (and assignee), parent status.

## Starting a subtask

Move the subtask to the `started` column. If its `startDate` is empty, set it to today. If the user says the real start differs from the planned `startDate`, ask whether to change it. When a subtask finishes after its `dueDate`, mention it in the summary to the user; do not silently change the date. Set the parent to `started` if it is not already.

## Closing the parent

When **all** subtasks are `completed` (or `canceled` with the user's agreement), move the parent to `completed` and add a one-line summary comment. If any subtask is still open, do not close the parent.

## Blocked / returning work

If the receiver finds the Spec incomplete: leave the receiver where it is, `add_comment` on the **sender** `[Spec]` with the specific questions (Thai), move the sender back to `started`, and tell the user. Do not edit the Spec content yourself.

## Cross-Issue dependencies

When a Frontend subtask waits on a Backend subtask in a **different** Issue (e.g. FM-23 waits on FM-19), use `link_issues` to record "blocked by" if the user wants it, and mention the dependency in the Frontend checklist line (`หลัง FM-xx เสร็จ`).
