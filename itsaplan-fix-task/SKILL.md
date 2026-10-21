---
name: itsaplan-fix-task
description: Record a BA fix list for existing work as one parent "[FIX] ..." Bug Issue plus [Database]/[Frontend]/[Backend] subtasks, then hand off to Dev. Use for "แก้ bug", "[FIX]", "รายการที่ต้องแก้". Read AGENT.md first.
---

# Itsaplan — Fix Task

For fixing existing work, not new features. Shared rules (ids, Thai, assignees, dates): `AGENT.md`.

## 1. Gather (ask BA one question at a time)
1. มีอะไรต้องแก้บ้าง (each = one numbered item)
2. Per item: หน้า/route, ตอนนี้ (อาการ), ควรเป็น, ตัวอย่างข้อมูล/ขั้นตอน (ถ้ามี), ต้องแก้ DB/backend ไหม (UI-only = frontend only)

Vague item ("แก้ UI") → ask what is wrong and what it should show; don't guess.

## 2. Subtasks
- `[Database]`: any item changes tables/columns/indexes/seed
- `[Backend]`: any item needs endpoint, server rule, data fix
- `[Frontend]`: any item changes React app

No `[Design]`/`[Spec]`; the parent list is the spec.

## 3. Resolve
`get_project` (Backlog, types `Bug`/`Task`, labels). `search_issues` for an existing `[FIX]` on the same page.

## 4. Parent
Backlog, type `Bug`, title `[FIX] ชื่อหน้า/งาน (/route)`, `startDate` always set, `dueDate`/`priority` if known. Description:
```
## รายการที่ต้องแก้
1. <สิ่งที่ต้องแก้>
   - หน้า: /route
   - ตอนนี้: ...
   - ควรเป็น: ...
   - ฐานข้อมูล/backend: ใช่ / ไม่ใช่
```

## 5. Subtasks
`parentId` = parent id; type `Task`; Backlog; labels `Frontend`/`Backend` (none for Database); title `[Kind] [FIX] ชื่อหน้า/งาน`; dates = parent's unless user says otherwise; description placeholder "แผนงานจะเสนอตอนเริ่มทำ — ดูรายการที่ต้องแก้ใน [FIX] หลัก" (Dev writes checklist per `itsaplan-dev-delivery`); assignee per `AGENT.md` after confirmation.

## 6. Verify and hand off
`get_issue` parent; report table (`FM-xx`, title, kind, assignee, skipped + reason). Then BA hands off per `itsaplan-handoff`: open first existing subtask in order `[Database]` → `[Backend]` → `[Frontend]` as `unstarted`.
