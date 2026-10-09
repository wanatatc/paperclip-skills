---
title: Backend - Database & EF Core Reverse Engineer
topic: Backend
source: 00_Inbox/SIAM SMILE Web Spec 2.0.1 - Backend.md
---

# Backend: Database & EF Core Reverse Engineer

**สรุปสำหรับ AI:** ออกแบบ DB ก่อน → Reverse Engineer ด้วย **EF Core Power Tools** (ได้ Model + DbContext). ห้ามแก้ไฟล์ที่ Generate โดยตรง → ใช้ `Partial` class แทน. ชื่อ DB ดู database-naming.md

## ขั้นตอน Reverse Engineer
1. ติดตั้ง EF Core Power Tools ใน Visual Studio
2. คลิกขวา Project → `EF Core Power Tools` → `Reverse Engineer`
3. Choose Database Connection → เลือก/Add connection string → EF Core version = **6.0**
4. Choose Database Objects → เลือกตารางที่ใช้
5. Choose Your Settings:
   - Context Name: `AppDbContext` (ถ้า DB เดียว) / ตามชื่อ DB ถ้าหลายตัว
   - Namespace: ตาม project
   - EntityTypes path: `Models`
   - DbContext path: `Data`
   - Naming: ✔ Pluralize/singularize (English), ✔ Use table & column names directly, ✔ Use DataAnnotations

## เปลี่ยนชื่อ Class / Field
- ที่หน้า Choose Database Objects กด `F2` เปลี่ยนชื่อ → ระบบสร้าง `efpt.renaming.json` ให้
- ถ้าใช้วิธีนี้ ต้อง **เอา option "Use table and column names directly" ออก**

## Relationship (Partial)
ถ้าไม่มี FK ใน DB (เช่น Microservice) → สร้าง partial class แล้วใช้ Fluent API:
```csharp
// Models/Partial/Agent.cs
public partial class Agent { public virtual ICollection<CallResult> CallResults { get; set; } }

// Data/Partial/AppDbContext.cs
partial void OnModelCreatingPartial(ModelBuilder modelBuilder) {
    modelBuilder.Entity<Agent>(e => e.HasMany(x => x.CallResults)
                                     .WithOne(x => x.Agent)
                                     .HasForeignKey(x => x.AgentId));
}
```

## Stored Procedure
Reverse Engineer ไม่ได้ property ของ SP → สร้าง partial model เพิ่ม property เอง (`Models/Partial/usp_Agent.cs`)

## หลาย Database ในโปรเจกต์เดียว
1. เปลี่ยนชื่อ `efpt.config.json` → `efpt.[ชื่อDBหลัก].config.json`
2. Reverse ใหม่: Context Name = `[ชื่อDB]Context`, EntityTypes path = `Models/[ชื่อDB]`
3. เปลี่ยนชื่อไฟล์ config ที่ได้ → `efpt.[ชื่อDB].config.json`
4. ต้องตรวจว่า Context name และ Models folder **ไม่ซ้ำกัน** ไม่งั้นไฟล์ทับกัน
5. ใน `Startup.cs` เพิ่ม `AddDbContext`, ใน `appsettings.json` เพิ่ม connection string

## Model folders
- `Base` — model พื้นฐานของระบบ
- `Partial` — ส่วนขยายของ model ที่ generate (แก้ไขตรงนี้แทนไฟล์ generate)
- `Response` — model สำหรับ response
