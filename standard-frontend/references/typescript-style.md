---
title: Frontend - TypeScript Style
topic: Frontend
source: 00_Inbox/SIAM SMILE Web Spec 2.0.1 - Frontend.md
---

# Frontend: TypeScript Style

**สรุปสำหรับ AI:** Base = Google TypeScript Style Guide. เฉพาะกฎที่ SSD เพิ่มหรือย้ำไว้ด้านล่างเท่านั้น

## Standard ที่ใช้: Google TypeScript Style Guide
ทุกอย่างที่ไม่ได้ระบุในไฟล์นี้ → ทำตาม Google TS Style Guide

## กฎของ SSD
- **ไฟล์:** import เรียง a-z → type ของ Props/State → component หลัก → sub-component/helper
- **ตัวอักษร:** ASCII เท่านั้น, ไม่มี `_` นำหน้า/ต่อท้ายชื่อ (รวม private)
- **Case:**
  - PascalCase: class, interface, type, enum, component, type parameter
  - camelCase: variable, param, function, method, property, import alias (`import * as fooBar`)
  - CONSTANT_CASE: global constant, enum value
- **คำย่อ:**
  - หลายคำ → ตัวใหญ่ทั้งหมด: `DTO`, `API`, `HTML`, `SMI`
  - 1 คำ → PascalCase: `Id`, `Cust`, `Doc`
  - camelCase ที่ขึ้นต้นด้วยคำย่อ → ตัวเล็กทั้งหมดในพยางค์แรก: `htmlDoc`, `xmlDTO`, `customerId`
- **ชื่อ:** สื่อความหมาย ห้ามย่อคลุมเครือ (`n`, `nErr`, `cstmrId`, `pcReader`) ห้าม Hungarian (`kSecondsPerDay`)
- **Interface:** ห้ามใส่ prefix `I` หรือ suffix `Interface`
- **Type annotation:** ใส่เฉพาะที่จำเป็น. ประกาศ type เสมอเมื่อเป็น object literal (ยกเว้น class)
- **Nullable:** ใช้ optional `?:` มากกว่า `|undefined`. ถ้า type ซ้ำให้ประกาศ alias แยก
- **Array:** `T[]` สำหรับทั่วไป, `Array<T>` เมื่อซับซ้อน (เช่น `Array<{n: number}>`)
- **Index signature:** ตั้งชื่อ key ให้มีความหมาย: `{[userName: string]: number}`
- **ห้าม:** `any` (ใช้ `unknown` + type guard), `{}` (ใช้ `unknown` หรือ `Record<string, T>`)
- **Tuple:** ใช้เมื่อคืนค่าเป็นคู่ เช่น `[string, string]`
- **Comment:**
  - หลายบรรทัด (ไม่ใช่ JSDoc) → `//` ทีละบรรทัด ห้าม `/* */`
  - JSDoc → `/** */`, tag ละ 1 บรรทัด, เนื้อหาเป็น Markdown
