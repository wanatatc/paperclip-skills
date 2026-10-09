---
title: Frontend - RPA & Automate Test Friendly
topic: Frontend
source: 00_Inbox/SIAM SMILE Web Spec 2.0.1 - Frontend.md
---

# Frontend: RPA & Automate Test Friendly

**สรุปสำหรับ AI:** ทุกหน้าจอต้องหา element ได้ด้วย `name`/`id` เพื่อให้ RPA และ automate test ทำงานได้ — **เป็นข้อบังคับ** ไม่ใช่ optional

## กฎ
1. Standard form component ใช้ได้ตามปกติ (ไม่ต้องแก้)
2. `Button` → ต้องมี attribute `name`
3. `Label`, `Typography` ที่แสดงผลลัพธ์ → ต้องมี `name`
4. Component ที่ render แบบ dynamic → ต้องมี attribute `id`
5. Formik component ที่สร้างเอง → ต้องตั้งทั้ง `name` และ `id` (ดู formik-forms.md)

```tsx
<Button name="btnSearch" onClick={handleSearch}>ค้นหา</Button>
<Typography name="lblTotal">{total}</Typography>
```
