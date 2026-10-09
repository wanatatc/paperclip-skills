---
title: Frontend - Standard Packages
topic: Frontend
source: 00_Inbox/SIAM SMILE Web Spec 2.0.1 - Frontend.md
---

# Frontend: Standard Packages

**สรุปสำหรับ AI:** ใช้ package ตามรายการนี้. ถ้าต้องการ package นอกรายการ **ต้องปรึกษาหัวหน้างานก่อน** (กันความหลากหลายเกินไป)

## ต้องใช้ (Required)
| Package | ใช้ทำอะไร |
|---|---|
| vite 4.3.9 | build tool |
| react 18.2.0 | UI |
| @mui/material ^5.13.4 | UI component |
| @mui/x-date-pickers ^6.9.0 | date picker |
| @fortawesome/react-fontawesome 6.4.0 | icon |
| @tanstack/react-query ^4.29.17 | server state |
| axios 1.4.0 | HTTP |
| dayjs 1.11.8 | date |
| formik 2.4.1 | form |
| mui-datatables 4.3.0 | table |
| oidc-client-ts 2.2.4 | OIDC login |
| react-dropzone 14.2.3 | upload file |
| react-helmet 6.1.0 | document head |
| react-number-format 5.2.2 | number format |
| react-redux 8.0.7 | redux binding |
| react-router-dom 6.12.1 | routing |
| react-timeago 7.1.0 | relative time |
| redux-persist 6.0.0 | persist redux state |
| sweetalert2 11.7.12 | popup |

## ควรใช้ (Recommended)
| Package | ใช้ทำอะไร |
|---|---|
| apexcharts ^3.35.0 | กราฟ |
| date-fns 2.29.3 | date |
| qrcode.react ^3.1.0 | QR Code |
| react-barcode ^1.4.6 | Barcode |
| zod ^3.22.4 | validation |
| zod-formik-adapter ^1.2.0 | zod + formik |
| html-to-image ^1.11.11 | HTML → image |
| js-file-download ^0.4.2 | download file |
| pdfmake ^0.2.10 | สร้าง PDF |
| uuid ^10.0.0 | UUID |

> ⚠ Version ใน `@tanstack/react-query` (v4) และ `react-query` ในตัวอย่างโค้ดไม่ตรงกัน — ดู react-query.md
