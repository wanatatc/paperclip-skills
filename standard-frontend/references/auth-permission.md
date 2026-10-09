---
title: Frontend - Authorization & Permission
topic: Frontend
source: 00_Inbox/SIAM SMILE Web Spec 2.0.1 - Frontend.md
---

# Frontend: Authorization & Permission

**สรุปสำหรับ AI:** Login ด้วย **OIDC** ผ่าน `oidc-client-ts`. ดึงข้อมูล user/สิทธิ์ด้วย `useAuth()` จาก `/src/app/modules/_auth`. เช็คสิทธิ์ด้วย `checkPermissions()`. สิทธิ์ทั้งหมดนิยามใน `PermissionList` (`/src/Const`)

## Setup
- ขอให้ Dev Lead สร้าง Google Sheet "API Registration" (copy จาก template) แล้วแชร์ให้ พี่วรรณ
- copy ค่าจาก sheet ไปใส่ `.env` (เช่น `VITE_SSO_*`) และ `Const.ts` (ส่วน `PermissionList`)

## useAuth()
```ts
const {
  authTokens,        // string  JWT token
  isAuthenticated,   // boolean login แล้วหรือยัง
  permissions,       // string[] สิทธิ์ทั้งหมด
  roles,             // string[] role ทั้งหมด
  user,              // User (oidc-client-ts)
  userProfile,       // ข้อมูลพนักงาน (ดูด้านล่าง)
} = useAuth();
```

## checkPermissions
```ts
import { PermissionList } from "/src/Const";
import { checkPermissions, useAuth } from "/src/app/modules/_auth";

const { permissions } = useAuth();
const ok = checkPermissions(permissions, [PermissionList.employee_read, PermissionList.employee_write], "AND");
```
- ค่าที่สามเป็น `"AND"` | `"OR"` — ถ้าไม่ระบุ default = `"OR"`

## userProfile fields
`userId`, `userName`, `email`, `employeeId`, `employeeCode`, `personId`, `fullName`, `firstName`, `lastName`, `branchId`, `branchDetail`, `areaId`, `areaDetail`, `departmentId`, `departmentDetail`, `teamId`, `teamDetail`, `positionId`, `positionDetail`

> `permissions` ยังใช้กับ route (`permissions` / `condition` ใน RouteObject) และ side menu ได้ — ดู routing-menu.md
