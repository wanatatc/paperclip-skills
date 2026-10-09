---
title: Backend - Database Naming (SSD)
topic: Backend
source: 00_Inbox/SIAM SMILE Web Spec 2.0.1 - Backend.md
---

# Backend: Database Naming (SSD)

**สรุปสำหรับ AI:** Base = Microsoft SQL Server naming (PascalCase, ไม่ย่อ ยกเว้นคำที่รู้จักดี เช่น `Id`, `Ftp`, `Dcr`). ที่ SSD เพิ่ม: ชื่อตารางเอกพจน์, คอลัมน์พื้นฐานมาตรฐาน, SP รูปแบบ pagination

## ทั่วไป
- PascalCase, คำเต็มภาษาอังกฤษ ห้ามย่อ (`EmployeeName` ✔ / `EmpName` ✘, `UserId` ✔ / `UsrId` ✘)

## Table
- เอกพจน์: `Employee`
- ตารางย่อย: ต่อท้าย `Header` / `Detail` (เช่น `OrderHeader`, `OrderDetail`)

## Column
- `datetime` → ใช้ `datetime2` เท่านั้น
- Id / Name ขึ้นต้นด้วยชื่อตาราง: `EmployeeId`, `EmployeeName`
- คอลัมน์พื้นฐานที่ทุกตารางควรมี: `IsActive`, `CreatedByUserId`, `CreatedDate`, `UpdatedByUserId`, `UpdatedDate`

## Stored Procedure
- ขึ้นต้น `usp_` แล้วตามด้วย PascalCase: `usp_Agent...`
- Output พื้นฐาน: `IsResult` (bit, 1=สำเร็จ/0=ไม่สำเร็จ), `Result` (nvarchar: SUCCESS | FAIL | ค่า), `Msg` (nvarchar)
- Pagination input: `IndexStart` (int), `PageSize` (int, 10–50), `SortField` (nvarchar), `OrderType` (nvarchar), `SearchDetail` (nvarchar)
- Pagination result column: `TotalCount` (int)
