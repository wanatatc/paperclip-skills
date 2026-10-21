---
name: itsaplan-dev-delivery
description: Guide a Dev through an Itsaplan [Database]/[Frontend]/[Backend] subtask - review Spec, propose plan for approval, keep a status block, resume, finish. Use when a Dev starts, resumes, or finishes such a subtask. Read AGENT.md first.
---

# Itsaplan — Dev Delivery

These subtasks are delivery task lists. The tracker records what is done/missing so any session can resume — not how code was written. Shared rules: `AGENT.md`.

## Stop and ask when
- `[Spec]` missing or has open "คำถามที่ยังค้าง" → tell user, don't plan. (`[FIX]` parent: use "รายการที่ต้องแก้" as the spec.)
- `[Design]` unclear for `[Frontend]`.
- A decision belongs to the Dev (schema, endpoint naming, library) → put in plan as a question.

## 1. Start
1. Read parent, `[Design]`/`[Spec]` (`get_issue`, `list_attachments`, `view_issue_images`), sibling status blocks/comments.
2. **Review to Dev first** (Thai, 3–6 lines): what this layer must deliver + unclear points. Wait for confirmation.
3. **Propose plan** before writing: small, verb-first checklist items (one session each), cite acceptance criterion (`AC-2`), last item = verification. Scale to work: one-liner needs no plan; schema/permission/multi-module changes get risks + open questions.
4. After Dev approves: `update_issue` description with `- [ ]` items, move to `started`.

## 2. Work
One status block at top of description, edited in place (not new comments):
```
## สถานะ
- เสร็จแล้ว: ...
- ยังเหลือ: ...
- ติดขัด / คำถาม: ... (รอใคร)
- ตัดสินใจแล้ว: ... (พร้อมเหตุผลสั้น)
- วิธีตรวจ: ...
```
- Tick only after verified (build/lint/test or seen running).
- Record what/status; how only if non-obvious (decision, constraint, trap). Link commits/PRs; no diffs/logs/secrets/real customer data.
- New fact changing plan → "ติดขัด / คำถาม" + comment on `[Spec]`; never edit Spec yourself.
- Keep it readable in <1 min; compress old "เสร็จแล้ว".

## 3. Resume
Read status block + checklist, then Spec acceptance criteria (or FIX list), then git log. Continue from "ยังเหลือ"; don't redo ticked items.

## 4. Finish
1. All items ticked or dropped with reason in "ตัดสินใจแล้ว".
2. Add hand-back: delivered, AC checked and how, what was not checked and why.
3. Move to `completed`, hand off per `itsaplan-handoff`.

When all delivery subtasks are done, run one end-to-end acceptance check before closing the Issue.
