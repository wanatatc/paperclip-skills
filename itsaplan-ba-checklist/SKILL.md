---
name: itsaplan-ba-checklist
description: Attach the BA document checklist (big project or small enhance) to an Itsaplan Initiative via one "[BA] ..." Task Issue with a ticked checklist. Use for "BA checklist", "เช็คลิสต์เอกสาร BA", new software-development initiative with BA/Dev involved.
---

# Itsaplan — BA Checklist

Tracks the project-level BA documents/sign-offs for one Initiative. Not per-page work (that is `itsaplan-create-task`). Itsaplan checklists attach to Issues only, so each Initiative gets **one** Issue `[BA] <ชื่อ initiative>` that holds the checklist.

## Rules
- Resolve ids with `get_project` (key e.g. `MP`); never invent or reuse ids across projects. Pick columns by `stateType`, types by name.
- All text written to Itsaplan in Thai. Ask when unclear; never delete (use `archive_issue`); no secrets/real customer data.
- Ask before creating anything. Only for software-development Initiatives (new system, new feature, change with BA/Dev involved) — skip knowledge/ops-only topics.

## Ask when
- Which Initiative (`list_initiatives`); none fits → ask whether to create one first.
- Size: **ใหญ่** (Project/Enhance ใหญ่) or **เล็ก** (Enhance เล็ก) if unclear.
- Assignee: propose per `itsaplan-create-task` "Team & assignees" (BA owns it), confirm before assigning.
- Dates: start (none → today); due optional, never before start.

## 1. Check duplicates
`search_issues` for `[BA]` + initiative title (in that project). If one exists, report it and stop unless the user wants to add items.

## 2. Issue
`create_issue`: type `Task`, Backlog column, `initiativeId` = chosen initiative, `startDate` set, title `[BA] <ชื่อ initiative>`, description one line: "Checklist เอกสาร BA ของ initiative นี้ (ขนาด ใหญ่/เล็ก)".

## 3. Checklist
`create_checklist` then `create_checklist_item` per row (items in this order).

**ใหญ่** — title `BA Checklist (Project ใหญ่)`; every item Musthave:
RFP · MOM · WorkFlow · UX/UI · Requirement Sign Off · Functional Specification · Test Scenario/Test Case · UAT Sign Off · Manual Documents · IT Announcement · Project Management

**เล็ก** — title `BA Checklist (Enhance เล็ก)`; Musthave: CR (ใบงาน), UAT Sign Off, IT Announcement, Project Management. Add the rest as `... _(nice to have)_`:
RFP · MOM · WorkFlow · UX/UI · Requirement Sign Off · Test Scenario/Test Case · Manual Documents

## 4. Ticking later
Tick items with `update_checklist_item` only when the user says the document is done (or points to its location). Never tick by guess. When all Musthave items are ticked, ask whether to move the Issue to `completed`.

## 5. Verify
`get_issue` the new Issue; report `KEY-xx`, size, item count, assignee.

## Not for
- Page/API subtasks → `itsaplan-create-task`; bug lists → `itsaplan-fix-task`; passing work on → `itsaplan-handoff`.
