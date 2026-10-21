# Paperclip Skills (SSD Standard)

ชุดสกิลสำหรับ Paperclip ที่รวมมาตรฐานการพัฒนาของ SSD จากเอกสาร SIAM SMILE Web Spec 2.0.1 และ AGENT.md สำหรับ agent ฝั่ง Backend และ Frontend

## โครงสร้าง

```
paperclip-skill/
├── standard-backend/        # สกิล: มาตรฐาน C# / ASP.NET Core
│   ├── SKILL.md
│   └── references/          # 10 ไฟล์
├── standard-frontend/       # สกิล: มาตรฐาน React / TypeScript
│   ├── SKILL.md
│   └── references/          # 13 ไฟล์
├── standard-git/            # สกิล: Git Flow, branch, commit message, release
│   ├── SKILL.md
│   └── references/          # git-conventions.md
├── standard-database/       # สกิล: ตั้งชื่อ DB, EF Core Reverse Engineer
│   ├── SKILL.md
│   └── references/          # 2 ไฟล์
├── itsaplan/                # สกิล: workflow Itsaplan (1 Issue = 1 UI)
│   ├── SKILL.md
│   └── references/          # 4 ไฟล์ (initial-project, create-task, handoff, dev-delivery)
└── agents/
    ├── backend/AGENT.md     # ใช้ standard-backend, standard-database, standard-git
    └── frontend/AGENT.md    # ใช้ standard-frontend, standard-git
```

## สกิลแต่ละตัว

| สกิล | ใช้เมื่อ |
|---|---|
| `standard-backend` | เขียนหรือรีวิวโค้ด C# ฝั่ง API เช่น ตั้งชื่อ, Controller, Service, DTO, AutoMapper, Serilog, Options, Quartz, RestSharp |
| `standard-frontend` | เขียนหรือรีวิวโค้ดเว็บ เช่น ตั้งชื่อ TS/React, โครง module, route, .env, login/สิทธิ์, Redux, NSwag/Axios, React Query, Formik, RPA attribute |
| `standard-git` | ตั้งชื่อ branch, เขียน commit message, เลือก Git Flow, SemVer, release-it, PR |
| `standard-database` | ตั้งชื่อ table, column, SP, pagination, EF Core Reverse Engineer, Partial model |
| `itsaplan` | ใช้ Itsaplan (project tracker) ตามหลัก "1 Issue = 1 UI" เช่น ตั้งค่าโปรเจกต์ใหม่, สร้าง task พร้อม subtask (Design/Spec/Database/Frontend/Backend), ส่งต่องานระหว่าง BA กับ Dev, วางแผนและอัปเดตสถานะงาน Dev |

## กฎหลักที่ทุกสกิลยึดร่วมกัน
- ใช้ Template ของ SSD เป็นฐานสำหรับงานในเครือ
- ห้ามเพิ่ม package นอกรายการที่กำหนดโดยไม่ปรึกษาหัวหน้า
- ห้าม log หรือส่งข้อมูลส่วนบุคคลและความลับ
- ห้ามแก้ไฟล์ที่ EF Core Power Tools generate โดยตรง ใช้ Partial class แทน
- ห้าม rebase ให้ใช้ Pull Request แทน และห้าม Reset commit ที่ push แล้ว
- เมื่อกฎขัดกันหรือตัวอย่างไม่สมบูรณ์ ให้แจ้งผู้ใช้ ไม่เดาเอง

## AGENT.md
- **Backend:** ทำงานตาม `standard-backend` เป็นหลัก เปิด reference เฉพาะส่วนที่เกี่ยวข้องกับงาน
- **Frontend:** ทำงานตาม `standard-frontend` เป็นหลัก ทุก UI ที่ RPA/test ต้องใช้ต้องมี `name` หรือ `id`
