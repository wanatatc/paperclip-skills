---
title: Frontend - React Style
topic: Frontend
source: 00_Inbox/SIAM SMILE Web Spec 2.0.1 - Frontend.md
---

# Frontend: React Style

**สรุปสำหรับ AI:** ใช้ TSX เสมอ, function component, 1 component ต่อ 1 ไฟล์ (ยกเว้น stateless/pure เล็ก ๆ). กฎด้านล่างเป็น standard ของ React ทั่วไป ไม่ใช่ของ SSD โดยเฉพาะ — เน้นส่วนที่ SSD กำหนดเพิ่ม

## กฎของ SSD
- ห้าม `React.createElement` (ยกเว้นโปรเจกต์ที่ไม่ใช้ TSX)
- ไฟล์ component: `PascalCase.tsx`. ไฟล์ตัวหลักของโฟลเดอร์ใช้ `index.tsx` และตั้งชื่อ component = ชื่อโฟลเดอร์ (import ได้ `from "./Footer"`)
- ตัวแปรที่เก็บ instance ของ component: camelCase (`const reservationItem = <ReservationCard />`)
- ชื่อ Props: หลีกเลี่ยงชื่อ DOM prop (`style`, `className`) → ใช้ `variant` แทน
- Props type: `<ComponentName>Props`, ใช้ `type` (ไม่ใช้ interface), export เสมอ
- Prop name: camelCase (`userName`, `phoneNumber`) ไม่ใช่ `UserName` / `phone_number`
- Boolean prop: ส่งแค่ชื่อ (`<Foo hidden />`) ไม่ใส่ `={true}`
- `key`: ห้ามใช้ `index` ถ้ามี id ที่เสถียร → ใช้ `todo.id`
- Ref: ใช้ `ref={myRef}` ห้าม string ref
- Multi-element return: ใช้ `<>...</>` (Fragment shorthand) ห้าม `<React.Fragment>`
- Event handler ที่ต้องส่งค่าเพิ่ม: ใช้ arrow function ใน JSX

## Template component (รูปแบบมาตรฐาน)
```tsx
export type ComponentNameProps = { title: string };

const ComponentName = ({ title }: ComponentNameProps) => {
    return <div><h1>{title}</h1></div>;
};

export default ComponentName;
```
