---
title: Backend - Configuration (appsettings & Options)
topic: Backend
source: 00_Inbox/SIAM SMILE Web Spec 2.0.1 - Backend.md
---

# Backend: Configuration (appsettings & Options)

**สรุปสำหรับ AI:** ค่าทั้งหมดอยู่ `appsettings.json` เป็น section ตามฟีเจอร์. เข้าถึงด้วย **Options Pattern** (`IOptions<T>`) — ห้าม hard-code ใน Startup

## Sections หลักของ appsettings.json
| Section | ความหมาย |
|---|---|
| `Project` | `Title`, `CleanTitle` (ไม่มี space), `Description`, `Version` |
| `Masstransit` | `EnableRabbitMQ` + `RabbitMQSetting`, `EnableKafka` + `KafkaSetting` |
| `OAuth` | `EnableOAuth`, `Scopes` |
| `Quartz` | `EnableQuartz`, `Jobs` (ชื่อ Job → cron) — ดู hosted-service-quartz.md |
| `Serilog` | `Using`, `WriteTo` (Console / File / MSSqlServer), `Properties.Application` — ดู logging.md |

## Options Pattern
เหตุผล: แต่ละ service รู้เฉพาะ config ที่ตัวเองใช้ (encapsulation / decouple)

1. ตั้งค่าใน appsettings:
```json
"Position": { "Title": "Editor", "Name": "Joe Smith" }
```
2. สร้าง class (ต้อง public, non-abstract, constructor ไม่รับ parameter, property เป็น `{ get; set; }` — **ห้ามใช้ field**):
```csharp
public class PositionSetting
{
    public const string Position = "Position";
    public string Title { get; set; }
    public string Name { get; set; }
}
```
> Tip: Visual Studio → Edit → Paste Special → Paste JSON as Classes
3. Register ใน `ProjectSetup.cs`:
```csharp
services.Configure<PositionSetting>(configuration.GetSection(PositionSetting.Position));
```
4. Inject `IOptions<T>` ใน service แล้วใช้ `.Value`:
```csharp
public TestService(IOptions<PositionSetting> options) { _options = options; }
var value = _options.Value.Title;
```
