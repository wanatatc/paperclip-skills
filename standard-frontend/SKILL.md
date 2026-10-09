---
name: standard-frontend
description: มาตรฐานการเขียน Frontend ของ SSD (React + TypeScript + Vite + MUI) ใช้เมื่อต้องเขียนหรือรีวิวโค้ดฝั่งเว็บ เช่น ตั้งชื่อ TypeScript/React, โครงโฟลเดอร์ module, เพิ่มหน้า/route/เมนู, env (.env), login และสิทธิ์, Redux slice, เรียก API (NSwag/Axios), React Query, Formik, RPA/test attribute หรือ package ที่ต้องใช้ ไม่ใช้กับ Backend หรือ Git
---

# Standard Frontend (SSD)

ใช้ไฟล์ใน `references/` เฉพาะหัวข้อที่เกี่ยวข้องกับงาน อย่าโหลดทั้งหมด

## ตารางเลือกไฟล์

| เมื่อต้องการ | เปิดไฟล์ |
|---|---|
| IDE, extension, Prettier | `references/tooling.md` |
| ตั้งชื่อตัวแปร/type, comment ใน TS | `references/typescript-style.md` |
| เขียน component, props, key, ref | `references/react-style.md` |
| โครงโฟลเดอร์ module, ตั้งชื่อ module/page/component | `references/project-structure.md` |
| เพิ่มหน้า/URL, route permission, side menu | `references/routing-menu.md` |
| .env, เพิ่ม environment variable | `references/env-config.md` |
| login, useAuth, เช็คสิทธิ์, userProfile | `references/auth-permission.md` |
| Redux slice, rootReducer, hooks | `references/redux.md` |
| เรียก API, generate client (NSwag), Axios | `references/api-client-axios.md` |
| useQuery/useMutation, queryKeys | `references/react-query.md` |
| ฟอร์ม Formik, CustomFormik, wrapper MUI | `references/formik-forms.md` |
| name/id สำหรับ RPA และ automate test | `references/rpa-test-friendly.md` |
| package ที่ต้องใช้และเวอร์ชัน | `references/packages.md` |

## กฎหลักที่ต้องยึดเสมอ
- ใช้ TypeScript + TSX และ function component เสมอ ห้าม `React.createElement`
- ไฟล์ component เป็น PascalCase.tsx; ไฟล์หลักของโฟลเดอร์ใช้ `index.tsx` และตั้งชื่อ component = ชื่อโฟลเดอร์
- Props type ตั้งชื่อ `<Component>Props` ใช้ `type` และ export เสมอ; prop name เป็น camelCase
- ห้ามใช้ `any` และ `{}` ห้ามใช้ `index` เป็น key ถ้ามี id
- Prettier: tabWidth 4, printWidth 120, semi true, singleQuote false, trailingComma es5
- อ่าน env ผ่าน `src/const.ts` เท่านั้น ห้ามใช้ `import.meta.env` ใน component
- ทุก Button, Label/Typography ที่แสดงผลลัพธ์, และ dynamic component ต้องมี `name` หรือ `id` (สำหรับ RPA)
- ห้ามเพิ่ม package นอกรายการใน `packages.md` โดยไม่ปรึกษาหัวหน้า
- ใช้ Template ของ SSD เป็นฐาน; ไม่แก้โฟลเดอร์ `layout/`, `_common/`, `_auth/` ถ้าไม่จำเป็น

> หมายเหตุ: ตัวอย่างโค้ดบางจุดในต้นฉบับมีความไม่สอดคล้องของ package (เช่น react-query v4 กับรูปแบบ v3) ให้ยืนยันกับโค้ดจริงก่อนใช้
