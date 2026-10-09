---
title: Frontend - React Query Pattern
topic: Frontend
source: 00_Inbox/SIAM SMILE Web Spec 2.0.1 - Frontend.md
---

# Frontend: React Query Pattern

**สรุปสำหรับ AI:** ทุก server state ผ่าน React Query. เขียน hook ใน `<resource>.query.ts` (แยกตาม Controller ของ API) ใช้ `queryKeys` เป็น object และ callback `onSucessCallback` / `onErrorCallback` (สะกดตามต้นฉบับ) ที่ hook. ตั้ง `enabled` เมื่อ query ต้องรอ param

## กฎ
- ไฟล์: `order.query.ts` ต่อ Controller
- `queryKeys` = object ของชื่อ key (`orderGetAll`, `orderGetById`, ...)
- Query hook: `useXxxGetAll(request, onSuccess?, onError?)`, `useXxxById(id, ...)`
- Mutation hook: `useXxxCreate(onSuccess?, onError?)`, `useXxxUpdate(id, ...)`
- ใน `onSuccess`: ถ้า `!data.isSuccess` → เรียก `onErrorCallback(new Error(message || exceptionMessage || "Unknown error"))` แล้วค่อยเรียก success callback
- ใน `onError`: ส่งต่อ `onErrorCallback(error)`
- Options มาตรฐานที่ใช้บ่อย: `staleTime` 5 นาที, `cacheTime` 10 นาที, `refetchOnWindowFocus: true`, `refetchInterval` 1 นาที (ใส่เฉพาะที่จำเป็น)

## โครงร่าง
```ts
export const queryKeys = { orderGetAll: "orderGetAll", orderGetById: "orderGetById" };

export const useOrderGetAll = (request: OrderGetAllRequest, onSuccessCallback?, onErrorCallback?) =>
  useQuery<OrderListServiceResponse, Error>(
    [queryKeys.orderGetAll, request],           // key รวม request เป็น dependency
    () => orderClient.orderGetAll(request.page, request.recordsPerPage),
    {
      staleTime: 1000 * 60 * 5,
      onSuccess: (data) => { /* check isSuccess → onErrorCallback, then onSuccessCallback */ },
      onError: (error) => onErrorCallback?.(error),
      enabled: !!request.leadSourceId,
    }
  );
```

> ⚠ ตรวจก่อนใช้: ตัวอย่างต้นฉบับใช้ import จาก `"react-query"` และ signature แบบ `useQuery(key, fn, options)`, แต่ package ที่กำหนดคือ `@tanstack/react-query` ^4 — ควรใช้ `@tanstack/react-query` และ object form `useQuery({ queryKey, queryFn, ... })` (ใน v5 `cacheTime` ถูกเปลี่ยนเป็น `gcTime`) — ยืนยันเวอร์ชันที่ใช้จริงก่อน
