---
title: Frontend - Environment Config
topic: Frontend
source: 00_Inbox/SIAM SMILE Web Spec 2.0.1 - Frontend.md
---

# Frontend: Environment Config

**สรุปสำหรับ AI:** Standard ของ SSD (ไม่ใช่ของ Vite ทั่วไป): env ทุกตัวต้องขึ้นต้น `VITE_`, ประกาศ type ที่ `src/vite-env.d.ts`, ตั้งค่าใน `.env*`, และ **ห้ามใช้ `import.meta.env` โดยตรง** → ใช้ `window.__CONST__ENV__` ผ่าน `src/const.ts`

## ไฟล์ .env (ลำดับความสำคัญ)
| ไฟล์ | ใช้เมื่อ |
|---|---|
| `.env` | ค่า base ทุกระบบ (build จะถูกทับด้วยไฟล์ด้านล่าง) |
| `.env.dev` | Development |
| `.env.uat` | UAT |
| `.env.production` | Production |
| `.env.local` | เครื่องตัวเองเท่านั้น |

## เพิ่ม env ใหม่
1. ประกาศ type ใน `src/vite-env.d.ts`:
```ts
interface ImportMetaEnv {
    readonly VITE_DEMOPOSAPI_URL: string;
}
```
2. ใส่ค่าใน `.env*` (เช่น `VITE_API_URL = "..."`)
3. export ผ่าน `src/const.ts` แล้ว import จาก `Const` ใน component (ห้ามอ่าน `window.__CONST__ENV__` ตรงใน component)
```ts
// src/const.ts
export const { VITE_BASE_URL, VITE_API_URL } = window.__CONST__ENV__;
// component
import { BASE_URL, API_URL } from "../Const";
```

## ค่า env มาตรฐานของ Template (ตัวอย่าง)
- `VITE_APP_NAME`, `VITE_APP_VERSION`, `VITE_APP_DESCRIPTION`
- `VITE_BASE_URL`, `VITE_API_URL`, `VITE_APIGW_BASEURL`
- `VITE_SSO_ISSUER`, `VITE_SSO_CLIENT_ID`, `VITE_SSO_SCOPE` (ใช้กับ OIDC — ดู auth-permission.md)

> ⚠ ในเอกสารต้นฉบับ ชื่อ global เขียนไม่สม่ำเสมอ (`__CONST__ENV__` / `CONST__ENV`) — ไฟล์นี้ใช้ `window.__CONST__ENV__` ควรตรวจกับโค้ดจริงของ Template ก่อนใช้งาน
