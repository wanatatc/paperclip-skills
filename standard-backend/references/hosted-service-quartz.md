---
title: Backend - Hosted Service (Quartz Cron Job)
topic: Backend
source: 00_Inbox/SIAM SMILE Web Spec 2.0.1 - Backend.md
---

# Backend: Hosted Service (Quartz Cron Job)

**สรุปสำหรับ AI:** งานตามเวลาใช้ **Quartz.NET**. ทำเป็น class `*Job` ที่ implement `IJob` ใน `HostedServices/`. ลงทะเบียนใน `ConfigQuartz` (ProjectSetup.cs) และกำหนดเวลาใน `appsettings.json` ด้วย cron

## ขั้นตอน
1. **สร้าง Job** ใน `HostedServices/` ชื่อไฟล์ลงท้าย `Job` (เช่น `BatchJob.cs`):
```csharp
[DisallowConcurrentExecution]
public class BatchJob : IJob
{
    public Task Execute(IJobExecutionContext context)
    {
        // งานที่ต้องทำ
        return Task.CompletedTask;
    }
}
```
2. **ลงทะเบียนใน `ConfigQuartz`** (ProjectSetup.cs):
```csharp
q.AddJobAndTrigger<LoggerRetentionJob>(quartzSetting);
q.AddJobAndTrigger<SyncUserJob>(quartzSetting);
```
3. **ตั้ง cron ใน appsettings.json** — key = ชื่อ class Job ที่ register ใน `ConfigQuartz` (เช่น `LoggerRetentionJob`):
```json
"Quartz": {
  "EnableQuartz": true,
  "Jobs": {
    "LoggerRetentionJob": "0 0 * ? * *",
    "SyncUserJob": "0 0 9,16 ? * * *"
  }
}
```

## Cron (รูปแบบ UNIX/Quartz)
- ลำดับ: `วินาที นาที ชั่วโมง วันของเดือน เดือน วันของสัปดาห์ [ปี]`
- ตัวอย่าง `0 0 8,14 * * *` = ทุกวัน 08:00 และ 14:00
- `0 0 * ? * *` = ทุกชั่วโมง (นาทีที่ 0)
- สร้าง/ตรวจ expression: freeformatter.com (Quartz cron generator)

## คุณสมบัติ Quartz (ที่ SSD ใช้)
- ตั้งเวลาล่วงหน้า/ทำซ้ำได้หลายรูปแบบ, รันเป็น cluster ได้, ป้องกันความล้มเหลว/retry ได้
- เก็บคิวงานได้ทั้ง in-memory หรือ database
