---
title: Frontend - API Client (Axios + NSwag)
topic: Frontend
source: 00_Inbox/SIAM SMILE Web Spec 2.0.1 - Frontend.md
---

# Frontend: API Client (Axios + NSwag)

**สรุปสำหรับ AI:** วิธีหลักคือ **generate client ด้วย NSwag** (`npm run code-gen`). ถ้า NSwag ทำไม่ได้ค่อยเขียน Axios client เอง. ทุก response ของ backend ห่อด้วย `ServiceResponse` และต้องเช็ค `isSuccess` เสมอ

## วิธี 1 (หลัก): NSwag
1. ตั้ง `API_URL` ใน `.env` ให้ชี้ backend (Swagger อยู่ที่ `${API_URL}/swagger/v1/swagger.json`)
2. รัน `npm run code-gen` → สร้าง `src/api/api.ts`
3. เปลี่ยนชื่อไฟล์ตาม Standard เช่น `orderApi.client.ts`

## วิธี 2: เขียน Axios client เอง (เมื่อ NSwag ไม่รองรับ)
- ประกาศ Request/Response interface เอง หรือ import จากไฟล์ NSwag
- ห่อ error ด้วย `.catch` แล้วคืน `error.response` ถ้ามี, ไม่งั้น throw
- ใน `.then` ถ้า `!response.data.isSuccess` → `throw new Error(response.data.exceptionMessage)`

```ts
export const getOrderGetById = (id: number): Promise<OrderResponseDto> =>
  axios
    .get(`${API_URL}/order/${id}`)
    .catch((_error: any) => { if (_error.response) return _error.response; throw _error; })
    .then((response: AxiosResponse<OrderResponseDtoServiceResponse>) => {
      if (!response.data.isSuccess) throw new Error(response.data.exceptionMessage);
      return response.data.data;
    });
```
> หมายเหตุ: ตัวอย่างต้นฉบับ `getById` คืน `response.data` ทั้งก้อน แต่ `getAll` คืน `response.data.data` — ยึดตามที่ใช้งานจริงในโปรเจกต์

## ServiceResponse (รูปแบบ response มาตรฐาน)
`data?`, `isSuccess?`, `message?`, `code?`, `exceptionMessage?`, `serverDateTime?`, `totalAmountRecords?`, `totalAmountPages?`, `currentPage?`, `recordsPerPage?`, `pageIndex?`

## โครงสร้างไฟล์
- `src/api/<resource>Api.client.ts` (NSwag) หรือ `<resource>Api.ts` (เขียนเอง)
- `<resource>.query.ts` = React Query hooks (ดู react-query.md)
- API หลายตัว → subfolder ต่อ API (`src/api/sale/saleApi.client.ts`)
