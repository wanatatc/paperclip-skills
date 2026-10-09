---
title: Frontend - Forms (Formik + MUI)
topic: Frontend
source: 00_Inbox/SIAM SMILE Web Spec 2.0.1 - Frontend.md
---

# Frontend: Forms (Formik + MUI)

**สรุปสำหรับ AI:** ฟอร์มใช้ **Formik** (`useFormik`) + validation ด้วย `validate` หรือ `zod` (`zod-formik-adapter`). Template มี **MUI component ที่ wrap กับ Formik แล้ว** ใน `src/app/modules/_common/components/CustomFormik` → ใช้ตัวนี้ก่อน ถ้าไม่พอค่อยสร้างใหม่ตามรูปแบบด้านล่าง

## ใช้ Formik มาตรฐาน (ย่อ)
- `useFormik<Values>({ initialValues, enableReinitialize: true, validate, onSubmit })`
- `enableReinitialize: true` เมื่อ initialValues มาจาก API (react-query)
- Submit/Reset: `formik.handleSubmit()` / `formik.handleReset()`

## Component ที่ใช้ร่วมกับ Formik (ของ SSD)
- ใช้ `<FormikTextField name="name" formik={formik} fullWidth />` แบบเดียวกันทุก field
- ดูตัวอย่างจริงที่ Demo POS (ภายใน)

## สร้าง Formik wrapper เอง (เมื่อ component ของ Template ไม่พอ)
1. สร้างใน `src/app/modules/components/` (component กลางของโปรเจกต์)
2. Props: `name: string`, `formik: FormikProps<...>`
3. อ่านค่า: `formik.getFieldProps(name)`, `formik.getFieldMeta(name)`
4. เขียนค่า: `formik.setFieldValue`, `formik.setFieldTouched`, `formik.setFieldError`
5. ต้องตั้ง `name` และ `id` เสมอ (ใช้กับ RPA/automate test — ดู rpa-test-friendly.md)
