---
title: Frontend - Routing & Side Menu
topic: Frontend
source: 00_Inbox/SIAM SMILE Web Spec 2.0.1 - Frontend.md
---

# Frontend: Routing & Side Menu

**สรุปสำหรับ AI:** ใช้ React Router v6 (`react-router-dom@6.12.1`) ผสม Custom RouteObject ของ Template. กำหนด route ที่ `src/app/routes/Routes.tsx`, เมนูซ้ายที่ `src/app/routes/ASideMenuList.tsx`

## RouteObject (custom ของ Template)
| field | ความหมาย |
|---|---|
| `path` | URL |
| `title` | ชื่อหน้า |
| `element` | JSX ที่แสดง |
| `children?` | route ย่อย |
| `index?` | `true` = หน้าแรกของ parent |
| `permissions?` | สิทธิ์ที่ใช้เข้าหน้า (`PermissionList.*`) |
| `condition?` | `"AND"` / `"OR"` (default `"OR"`) |

กฎ:
- ถ้ามี `children` → parent **ห้าม** มี `index` หรือ `permissions`; ไปกำหนดที่ child ที่เป็น `index` แทน

```tsx
{ path: "/employee", title: "Employee", element: <EmployeePage />,
  permissions: [PermissionList.employee_read], condition: "AND" },
{ path: "/demo", title: "Demo", children: [
    { index: true, title: "Demo", element: <BlankPage body="Demo" /> },
    { path: ":id", title: "Demo id", element: <BlankPage body="Demo" /> },
] },
```

## ชื่อ Route (URL)
- ประโยคสั้น ไม่เกิน 70 ตัวอักษร
- ตัวพิมพ์เล็กทั้งหมด, ไม่มีเครื่องหมายวรรคตอน (ใช้ `-` แทน), ไม่มีเครื่องหมายกำกับเสียง
- ไม่มีนามสกุลไฟล์ (`/order` ไม่ใช่ `/order.tsx`)
- โครงสร้างเดียวกันทุก URL ที่เกี่ยวข้อง

## Side menu (`ASideMenuList.tsx`)
- เพิ่มเมนูใหม่ **หลัง** `ASideMenuListTopMenu` ใน `ASideMenuListDrawer > ASideMenuListContainer`
- Component:
  - `<MenuItem path icon text permissions? condition? />`
  - `<ParentMenu icon text>...</ParentMenu>` (กลุ่มเมนูที่มี subdirectory)
- ตัวอย่าง:
```tsx
<ParentMenu icon="home" text="Formik">
  <MenuItem path="/demo/formik" icon="home" text="Index" />
</ParentMenu>
```
