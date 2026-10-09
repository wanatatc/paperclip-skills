---
title: Frontend - Project Structure & Naming
topic: Frontend
source: 00_Inbox/SIAM SMILE Web Spec 2.0.1 - Frontend.md
---

# Frontend: Project Structure & Naming

**สรุปสำหรับ AI:** โค้ดอยู่ใน `src/app/`. แบ่งเป็น `api/`, `modules/`, `routes/`, และ `redux/` (อยู่ที่ `src/redux`). แต่ละ feature คือ 1 module ใน `modules/<Module>/`. ห้ามแก้ `layout/`, `modules/_common/`, `modules/_auth/` ถ้าไม่จำเป็น (มาจาก Template กลาง)

## โครงสร้าง
```
src/app/
├── api/                          # ไฟล์เรียก API ทั้งหมด
│   ├── orderApi.client.ts        # axios จาก NSwag (generated)
│   ├── orderApi.ts               # axios เขียนเอง (ไม่ใช้ NSwag)
│   ├── order.query.ts            # React Query hooks
│   └── sale/                     # ถ้ามีหลาย API: 1 folder ต่อ 1 API
│       ├── saleApi.client.ts
│       └── slip.query.ts
├── layout/                       # Template (ไม่ควรแก้)
├── modules/
│   ├── _common/                  # component กลางของ Template (ไม่ควรแก้)
│   ├── _auth/                    # auth ของ Template (ไม่ควรแก้)
│   ├── components/               # component กลางของโปรเจกต์
│   │   ├── CommonProject.tsx
│   │   ├── CommonProject.hook.tsx
│   │   └── index.ts              # export รายชื่อ component
│   └── Order/                    # 1 module = 1 feature
│       ├── components/
│       │   ├── OrderTable.tsx
│       │   ├── OrderTable.hook.tsx
│       │   └── index.ts
│       ├── pages/
│       │   ├── IndexPage.tsx     # หน้าแรกของ module (บังคับทุก module)
│       │   ├── OrderPage.tsx
│       │   ├── OrderPage.hook.tsx
│       │   └── index.ts
│       └── orderSlice.ts         # redux slice ของ module
├── routes/
│   ├── Routes.tsx
│   └── ASideMenuList.tsx
└── const.ts                      # ตัวกลาง env variable (ดู env-config.md)
```

## กฎการตั้งชื่อ
- **Module:** PascalCase, คำเดียวหรือคำที่เกี่ยวข้อง ไม่เกิน 3 คำ
- **Component:** PascalCase, ไม่เกิน 3 คำ. ลงท้ายตามประเภท:
  - `Container` = ห่อ component อื่น
  - `View` / `Card` = แสดงผล
  - `Form` = ฟอร์ม, `Table` = ตาราง
  - `List` / `Item` = รายการข้อมูล
  - Formik component ของ Template: `ProvinceSelect`, `EmployeeAutoComplete`, `InterestCheckboxGroup`, `GenderRadioGroup`
- **Custom hook:** แยกไฟล์ `*.hook.tsx`
- **Page:** PascalCase, ลงท้าย `Page`, หน้าแรกของ module = `IndexPage`
- **Redux slice:** ลงท้าย `Slice` เช่น `orderSlice.ts` (ถ้าซับซ้อนแยกเป็น `<module><Sub>Slice.ts`)
- **API client/query:** `<resource>Api.client.ts` / `<resource>.query.ts` (ชื่อไฟล์ตาม Controller ของ API)
