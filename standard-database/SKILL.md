---
name: standard-database
description: มาตรฐานฐานข้อมูล SQL Server ของ SSD ใช้เมื่อต้องตั้งชื่อ table/column/stored procedure, ออกแบบคอลัมน์พื้นฐาน, เขียน pagination SP, ทำ EF Core Reverse Engineer ด้วย EF Core Power Tools, เพิ่ม Partial model/relationship, หรือใช้หลาย database ในโปรเจกต์เดียว
---

# Standard Database (SSD)

## ตารางเลือกไฟล์

| เมื่อต้องการ | เปิดไฟล์ |
|---|---|
| ตั้งชื่อ table, column, stored procedure, pagination | `references/database-naming.md` |
| Reverse Engineer, Model/DbContext, Partial, หลาย DB | `references/database-ef-core.md` |

## กฎหลักที่ต้องยึดเสมอ
- SQL Server 2019 ขึ้นไป, Entity Framework Core 6 สำหรับเชื่อมต่อ
- ชื่อ PascalCase, คำเต็มภาษาอังกฤษ ห้ามย่อ (ยกเว้นคำที่รู้จักดี เช่น `Id`, `Ftp`, `Dcr`)
- Table ใช้คำนามเอกพจน์ ตารางย่อยต่อท้าย `Header` / `Detail`
- Column ประเภทเวลาใช้ `datetime2` เท่านั้น; Id/Name ขึ้นต้นด้วยชื่อตาราง
- ทุกตารางควรมี `IsActive`, `CreatedByUserId`, `CreatedDate`, `UpdatedByUserId`, `UpdatedDate`
- Stored procedure ขึ้นต้น `usp_`; output มี `IsResult`, `Result`, `Msg`; pagination ใช้ `IndexStart`, `PageSize`, `SortField`, `OrderType`, `SearchDetail` และคืน `TotalCount`
- ห้ามแก้ไฟล์ที่ EF Core Power Tools generate โดยตรง ให้ใช้ `Partial` class
- ก่อนขึ้น Production ถ้ามีการเปลี่ยนโครงสร้างหรือ SP ใหม่ ต้องมี Migration Script ให้ DBA
