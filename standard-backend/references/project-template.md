---
title: Backend - Project Template & Folder Structure
topic: Backend
source: 00_Inbox/SIAM SMILE Web Spec 2.0.1 - Backend.md
---

# Backend: Project Template & Folder Structure

**สรุปสำหรับ AI:** เริ่มจาก Template ของ SSD (GitHub Organization ของ SiamSmile). ห้ามเพิ่ม package นอกรายการโดยไม่ปรึกษาหัวหน้า

## Packages
**ต้องใช้:**
- Entity Framework Core (SQL Server) 6.0.16
- Swashbuckle.AspNetCore (Swagger)
- Serilog 6.1.0 (log)
- AutoMapper 12.0.1
- RestSharp 109.0.1
- Quartz 3.6.2 (scheduled job)
- prometheus-net 6.0.0

**ควรใช้:** MassTransit 7.2.4, NPOI 2.6.0, FluentValidation, CsvHelper

## Folder structure (ตาม dotnet standard)
| Folder | ใช้เก็บ |
|---|---|
| `Attributes/Auth` | attribute เรื่องสิทธิ์ (เช่น `ClaimPermission`) |
| `Attributes/Validation` | attribute เช็ค validation |
| `Clients` | API ภายนอก (RestSharp) |
| `Configurations` | option class ของ appsettings |
| `Consumers` | consumer รับข้อมูลจาก Message Queue |
| `Controllers` | API controller |
| `Data` | DbContext (EF Core) |
| `DTOs/Auth`, `DTOs/Pagination` | DTO เฉพาะกลุ่ม |
| `Exceptions` | custom exception |
| `Helpers` | helper / extension method |
| `HostedServices` | cron job (Quartz) |
| `Logs` | logging |
| `Middleware` | middleware |
| `Models/Base`, `Models/Partial`, `Models/Response` | entity และ response model |
| `Services/Auth`, `Services/Base` | service (`Auth` = ข้อมูลผู้ใช้, `Base` = ServiceBase) |
| `Startups` | startup |

## ไฟล์ตั้งค่าหลัก
- `AutoMapperProfile.cs` — ตั้งค่า mapping ระหว่าง Model กับ DTO
- `ProjectSetup.cs` — รวม setup ทั้งหมด แบ่งเป็น:
  - `ConfigDependency` — DI
  - `ConfigQuartz` — job
  - `ConfigRabbitMQ` — RabbitMQ
  - `ConfigKafka` — Kafka
