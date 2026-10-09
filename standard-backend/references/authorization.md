---
title: Backend - Authorization & Permission
topic: Backend
source: 00_Inbox/SIAM SMILE Web Spec 2.0.1 - Backend.md
---

# Backend: Authorization & Permission

**สรุปสำหรับ AI:** JWT (OAuth) → สิทธิ์อยู่ใน claim `permission`. ตั้งชื่อสิทธิ์ใน `Permission.cs`. ดึงข้อมูล user ผ่าน `ILoginDetailServices` (อ่านจาก claims ของ JWT). ใช้ `[Authorize]` และ `[ClaimPermission(...)]`

## Setup
- ขอให้ Dev Lead สร้าง Google Sheet "API Registration" (copy จาก template) แล้วแชร์ให้ พี่วรรณ
- copy ค่าจาก sheet → `appsettings.json` และ `Permission.cs` (ส่วน Permission)

## Permission.cs
```csharp
public class Permission
{
    public const string WalletUser = "wallet_api:user";
    public static AuthorizationPolicy WalletUserPermission = new AuthorizationPolicyBuilder()
        .RequireClaim("permission", WalletUser)
        .Build();
}
```
- ค่าสิทธิ์ = string const ใน class `Permission`
- Policy = `RequireClaim("permission", <const>)`

## ILoginDetailServices (ข้อมูล user)
- Inject เฉพาะ service ที่ต้องใช้ข้อมูล user
- `_login.GetClaim()` → ได้ object ที่มี:
  | Property | Type | ความหมาย |
  |---|---|---|
  | UserId | int | รหัสผู้ใช้ในระบบ |
  | EmployeeCode | string | รหัสพนักงาน |
  | Firstname | string | ชื่อ |
  | Lastname | string | นามสกุล |
  | BranchId | int | รหัสสาขา |
  | Branchname | string | ชื่อสาขา |
- `_login.CheckPermission(Permission.X)` → bool

## Attribute
- `[Authorize]` ที่ Controller → ต้อง login
- `[ClaimPermission(Permission.Admin, Permission.Manager)]` ที่ method → จำกัดสิทธิ์รายการ

> ⚠ ตัวอย่างต้นฉบับ `public async bool CanRead()` ไม่ใช่ syntax ที่ถูกต้อง (ต้องเป็น `public bool` หรือ `async Task<bool>`) และชื่อ interface ในเอกสารเขียนทั้ง `ILoginDetailService` และ `ILoginDetailServices` — ยืนยันกับโค้ดจริง
