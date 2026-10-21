# Dev Delivery: [Database] / [Frontend] / [Backend]

These three subtasks are **delivery task lists**, not design documents. The tracker is the middle layer between the Dev and the AI agent(s): it records what is done and what is missing so any new session can resume. It does not record how the code was written.

## Ask when

- The linked `[Spec]` is missing, or has open "คำถามที่ยังค้าง" → stop, tell the user, do not plan.
- The `[Design]` attachment/link is unclear and the work is `[Frontend]`.
- A planned sub-task needs a decision that is the Dev's (schema shape, endpoint naming, library) → put it in the plan as a question, do not decide silently.

## Phase 1 — Start: review, then propose a plan

Trigger: the Dev says they are starting a `[Database]`, `[Frontend]`, or `[Backend]` subtask.

1. Read the parent Issue, its `[Design]` and `[Spec]` subtasks (`get_issue`, `list_attachments`, `view_issue_images` if needed), and the status block / comments of sibling delivery subtasks.
2. **Review back to the Dev first** (short, Thai): what the Spec says this layer must deliver, in 3–6 lines, plus anything unclear or missing. Wait for the Dev to confirm or correct. Do not skip this even if the Spec looks complete.
3. **Propose a plan** as a checklist of sub-tasks for this layer and show it to the Dev *before* writing it into Itsaplan. Each item is small and checkable (one session's worth), starts with a verb, and where possible names the Spec acceptance criterion it satisfies (`AC-2`). Add a final item for verification (what will be run or looked at).
   - Scale the plan to the work: a one-line change needs no plan; a normal single-page layer gets a short checklist; a change touching schema, permissions, or several modules gets a fuller plan with risks and open questions.
4. Only after the Dev approves (or edits) the plan, write it into the subtask description with `update_issue` as `- [ ]` items, and move the subtask to the `started` column.

## Phase 2 — Work: keep the status block current

Each delivery subtask carries **one status block** at the top of its description, edited in place with `update_issue` (not appended as new comments):

```
## สถานะ
- เสร็จแล้ว: ...
- ยังเหลือ: ...
- ติดขัด / คำถาม: ... (ระบุว่ารอใคร)
- ตัดสินใจแล้ว: ... (บรรทัดเดียว พร้อมเหตุผลสั้นๆ)
- วิธีตรวจ: ...
```

Followed by the plan checklist. Rules:

- Tick a checklist item only after it has been verified (build/lint/test run, or looked at in the running app). Never tick on intention.
- Record **what** and **status**. Record **how** only when it is necessary for the next person: a non-obvious decision, a constraint, or a trap that cost time. Everything else lives in git history and the PR.
- Reference commits/PRs by link or short hash; do not paste diffs or logs.
- Never put secrets, credentials, or real customer data in an Issue.
- When a new fact changes the plan (a missing field, a Spec gap), add it to "ติดขัด / คำถาม" and comment on `[Spec]` so BA sees it; do not edit the Spec's business content yourself.
- Keep the status block short enough to read in under a minute. If it grows long, compress older "เสร็จแล้ว" lines.

## Phase 3 — Resume

When a new session (or a different agent) picks up the subtask: read the status block and checklist first, then the Spec's acceptance criteria, then git log for the branch. Continue from "ยังเหลือ"; do not redo ticked items without a reason.

## Phase 4 — Finish

1. Every plan item is ticked or explicitly dropped with a reason in "ตัดสินใจแล้ว".
2. Add a short hand-back to the status block: what was delivered, which acceptance criteria were checked and how, what was **not** checked and why (e.g. backend not ready, mobile not looked at).
3. Move the subtask to `completed` and hand off per `handoff.md`.

When all delivery subtasks of the Issue are done, do one end-to-end check of the Issue's acceptance criteria across layers before the Issue is closed.
