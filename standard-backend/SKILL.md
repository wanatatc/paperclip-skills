---
name: standard-backend
description: มาตรฐานการเขียน Backend ของ SSD (ASP.NET Core / C# / .NET 6) ใช้เมื่อต้องเขียนหรือรีวิวโค้ด C# ฝั่ง API เช่น ตั้งชื่อ class/method, โครง project และ Template, Controller, Service, DTO, AutoMapper, Logging (Serilog), Configuration (Options), งาน Quartz, หรือเรียก API ภายนอกด้วย RestSharp ไม่ใช้กับ Database (ใช้ standard-database) หรือ Git (ใช้ standard-git)
---

# Standard Backend (SSD)

ใช้ไฟล์ใน `references/` เฉพาะหัวข้อที่เกี่ยวข้องกับงาน อย่าโหลดทั้งหมด

## ตารางเลือกไฟล์

| เมื่อต้องการ | เปิดไฟล์ |
|---|---|
| Visual Studio, ตั้งชื่อ C#, จัดวางโค้ด, comment | `references/tooling-and-style.md` |
| Package ที่ต้องใช้, โครงโฟลเดอร์, ProjectSetup.cs | `references/project-template.md` |
| Model, DTO (ตั้งชื่อ, Flatten), Controller | `references/models-dtos-controllers.md` |
| Service + Interface, DI, pattern create/update | `references/services.md` |
| สิทธิ์ (Permission), ข้อมูลผู้ใช้, [Authorize] | `references/authorization.md` |
| appsettings.json, Options Pattern | `references/configuration.md` |
| Serilog, ระดับ log, ForContext, CollationId | `references/logging.md` |
| AutoMapper, Profile, ProjectTo | `references/automapper.md` |
| งานตามเวลา (Quartz, cron) | `references/hosted-service-quartz.md` |
| เรียก API ภายนอก (RestSharp, authentication) | `references/api-client-restsharp.md` |

## กฎหลักที่ต้องยึดเสมอ
- C# v10 บน .NET 6; ชื่อ method/class/property เป็น PascalCase, parameter/local เป็น camelCase, private field เป็น `_camelCase`, interface ขึ้นต้นด้วย `I`
- ทุก Service ต้องมี Interface คู่กันเสมอ
- Controller inject เฉพาะ Service interface และคืนค่าเป็น `ServiceResponse` ผ่าน `ResponseResult`
- Entity ห้ามส่งออกตรง ๆ ต้องแปลงเป็น DTO (ดูแลด้วย AutoMapper Profile เท่านั้น ห้ามใช้ MappingExtension)
- ห้าม log ข้อมูลส่วนบุคคล (เลขบัตร, เบอร์โทร, รหัสผ่าน) ถ้าจำเป็นต้อง mask ก่อนเสมอ
- ค่า config ทั้งหมดอยู่ใน appsettings.json เข้าถึงผ่าน `IOptions<T>` ห้าม hard-code
- ห้ามเพิ่ม package นอกรายการใน `project-template.md` โดยไม่ปรึกษาหัวหน้า
- ใช้ Template ของ SSD เท่านั้น (งานในเครือ)

> หมายเหตุ: ตัวอย่างโค้ดบางจุดในต้นฉบับยังไม่สมบูรณ์ ให้ยึดรูปแบบ ไม่ใช่คัดลอกไปคอมไพล์ทันที
