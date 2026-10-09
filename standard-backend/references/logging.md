---
title: Backend - Logging (Serilog)
topic: Backend
source: 00_Inbox/SIAM SMILE Web Spec 2.0.1 - Backend.md
---

# Backend: Logging (Serilog)

**สรุปสำหรับ AI:** ใช้ **Serilog** เท่านั้น. Structured log (`{@Object}`). **ห้าม log ข้อมูลส่วนบุคคล** (เลขบัตรประชาชน, เบอร์โทร, รหัสผ่าน, ความลับ) — ถ้าจำเป็นต้อง mask ก่อนเสมอ

## ระดับ Log
| Level | ใช้เมื่อ |
|---|---|
| Verbose | ติดตามละเอียดสุด อาจมีข้อมูลอ่อนไหว ปิดเป็นค่าเริ่มต้น ห้ามเปิดใน Production |
| Debug | debug/dev ระวังปริมาณใน Production |
| Information | เหตุการณ์สำคัญ ใช้ตรวจสอบระยะยาว |
| Warning | อาจมีปัญหาแต่ยังทำงานได้ |
| Error | ปัญหาที่แก้ไม่ได้ใน request/โปรแกรมปัจจุบัน |
| Fatal | วิกฤต เช่น ข้อมูลสูญหาย, disk เต็ม |

## แนวทางการลง Log
- Action ที่ **เปลี่ยนแปลงข้อมูล / สำคัญ** → Information อย่างน้อย 1 ครั้ง
- ทุก method ที่เข้าถึง → Debug ขึ้นไป (โดยเฉพาะ method ซับซ้อน)
- Body request/response → Trace หรือ Debug
- ห้ามลง PII / ความลับ; ถ้าหลีกเลี่ยงไม่ได้ → Mask

## การเรียกใช้
1. ตรงจาก Serilog: `Log.Information("Created {@User} on {Created}", user, DateTime.Now);`
2. Inject logger (เพื่อ test ได้):
```csharp
public ClassName(Serilog.ILogger? logger = null) { _logger = logger ?? Log.Logger; }
```

## ForContext
- เพิ่ม property ให้ log: `_logger.ForContext("ControllerName", name)`
- **CollationId**: ใช้ผูกหลาย log ของ flow ที่ซับซ้อนเข้าด้วยกัน
```csharp
var log = _logger.ForContext("CollationId", Guid.NewGuid());
log.Information("Start MethodName");
SubMethod1(log);   // ส่ง log ต่อไปให้ sub method ใช้ CollationId เดียวกัน
log.Information("End MethodName");
```
- ถ้ามี Unique Id ของ flow อยู่แล้ว ใช้ค่านั้นเป็น CollationId ได้
