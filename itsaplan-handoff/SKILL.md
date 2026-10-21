---
name: itsaplan-handoff
description: Pass work on in Itsaplan - close a finished subtask (column, assignee, summary comment) and open the next for the receiver (BA→Dev, Dev→Dev, FIX); close the parent when all done. Use for "ส่งต่องาน", "handoff", "ปิด subtask", "ส่งให้ Dev". Read AGENT.md first.
---

# Itsaplan — Handoff

Handoff = **move column + change assignee + summary comment**. Shared rules: `AGENT.md`.

## Order
`[Design]` (BA) → `[Spec]` (BA) → `[Database]` → `[Backend]` → `[Frontend]`. Skip missing subtasks; next existing one receives. Frontend may start early only against a user-confirmed Swagger contract. Roles/assignees per `AGENT.md`.

`[FIX]` parent: no Design/Spec; BA opens first existing subtask (`[Database]` → `[Backend]` → `[Frontend]`) with a comment pointing to the parent.

## Ask when
- Receiver: propose per `AGENT.md`, ask "จะใช้คนตามนี้เลยไหม?" before assigning.
- Sender has unchecked checklist items/blank Handoff fields → ask whether to proceed.
- Deliverable missing or location unclear.

## Procedure
1. Find subtask: `get_issue_by_number` or parent's `subtasks`.
2. Verify deliverable (else stop, tell user, don't close):
   - Design: attachment (`list_attachments`) or link/HTML in description/comment.
   - Spec: business Handoff filled (วัตถุประสงค์, flow, ข้อมูล, กฎ, บทบาท, กรณียกเว้น, AC) and no open questions. Don't require/add technical content; don't copy it into the summary as requirement.
   - Database/Backend/Frontend: checklist ticked; Backend Swagger matches real response.
3. Close sender: `update_issue` `columnId` = `completed`.
4. `add_comment` on sender (Thai):
   ```
   ส่งต่อ: <ชนิดงาน> → <ชนิดงานถัดไป> (FM-xx)
   สิ่งที่ส่งมอบ: <ลิงก์/ไฟล์/สรุปสั้นๆ>
   ที่ผู้รับต้องรู้: <ข้อตกลง, ข้อจำกัด, สิ่งที่ยังไม่ทำ>
   คำถามที่ยังค้าง: <ถ้ามี>
   ```
   `@username` only if user confirmed; post a short pointer on the receiver too.
5. Open receiver: `columnId` = `unstarted`, `assigneeUserId` once confirmed.
6. Parent in `backlog` → `started` when first subtask moves to `started` (not Todo).
7. Report: sender closed, receiver + assignee, parent status.

## Starting a subtask
Move to `started`; empty `startDate` → today; if real start differs from plan, ask. Finished after `dueDate` → mention to user, don't change silently. Set parent `started`.

## Closing parent
All subtasks `completed` (or `canceled` with user's OK) → parent `completed` + one-line comment. Otherwise leave open.

## Blocked / returning
Receiver finds Spec incomplete: leave receiver, `add_comment` on sender `[Spec]` with questions, move sender back to `started`, tell user. Never edit Spec content.

## Cross-Issue dependency
Frontend waiting on Backend in another Issue: `link_issues` "blocked by" if user wants; add checklist note `หลัง FM-xx เสร็จ`.
