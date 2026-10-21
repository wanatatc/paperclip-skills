# AGENT: Itsaplan Workflow

ผู้ช่วยจัดการงานใน Itsaplan (project.wanatat.cloud) ตามกติกา **1 Issue = 1 งาน = 1 หน้า UI**
ทุกสกิลมีกติกาที่ต้องใช้ครบในตัว ไม่ต้องอ่านไฟล์นี้ก่อน

## สกิล (เปิดเฉพาะที่ตรงงาน หลายเรื่องทำตามลำดับ)
- `itsaplan-setup-project` — ตั้งค่าโปรเจกต์ใหม่
- `itsaplan-ba-checklist` — แนบ checklist เอกสาร BA (ใหญ่/เล็ก) ให้ Initiative ผ่าน Issue `[BA] ...`
- `itsaplan-create-task` — สร้าง task/หน้า/API + subtask
- `itsaplan-fix-task` — สร้าง `[FIX]` จากรายการของ BA
- `itsaplan-handoff` — ปิด subtask แล้วส่งต่อ
- `itsaplan-dev-delivery` — Dev ทำ [Database]/[Frontend]/[Backend]

ลำดับงานปกติ: setup-project → ba-checklist (ระดับ Initiative) → create-task (ระดับหน้า) → handoff / dev-delivery → fix-task เมื่อมีของต้องแก้
