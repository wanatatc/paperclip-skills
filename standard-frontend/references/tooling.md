---
title: Frontend - Tooling & Formatting
topic: Frontend
source: 00_Inbox/SIAM SMILE Web Spec 2.0.1 - Frontend.md
---

# Frontend: Tooling & Formatting

**สรุปสำหรับ AI:** ใช้ VS Code + Chrome ตามรายการ และ format โค้ดด้วย Prettier ค่าตามด้านล่าง

## IDE / Extensions (แนะนำ)
- VS Code: Auto Close Tag, Auto Rename Tag, ESLint, Path Intellisense, Prettier, ES7+ React/Redux snippets (ใช้ `tsrafce` สร้าง component), Error Lens
- Chrome: Redux DevTools, React Developer Tools

## Prettier (ค่าของ SSD)
- `tabWidth: 4`, `printWidth: 120`, `semi: true`, `singleQuote: false`, `trailingComma: "es5"`, `endOfLine: "crlf"`

> ⚠ ตรวจก่อนใช้: `eslintIntegration`, `stylelintIntegration`, `editorconfig` ไม่ใช่ option ของ Prettier จริง (น่าจะเป็น config ของ IDE) — อย่าเอาไปใส่ `.prettierrc` ตรง ๆ โดยไม่ตรวจ
