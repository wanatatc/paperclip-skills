---
title: Backend - Models, DTOs & Controllers
topic: Backend
source: 00_Inbox/SIAM SMILE Web Spec 2.0.1 - Backend.md
---

# Backend: Models, DTOs & Controllers

**สรุปสำหรับ AI:** Entity ไม่ส่งออก API ตรง ๆ → ใช้ DTO เสมอ. Controller บางที่สุด (เรียก Service) และคืนค่าเป็น `ServiceResponse` ผ่าน `ResponseResult`

## Model
- Entity จาก Reverse Engineer → อยู่ `Models/`. เปลี่ยนแปลงผ่าน `Partial`
- Stored Procedure ที่ต้องการ property เพิ่ม → `Models/Partial/usp_*.cs`

## DTO
- คือ object ส่งข้อมูลระหว่าง Client ↔ Server ไม่เก็บ DB
- **การตั้งชื่อ** (ต่อท้ายตามวัตถุประสงค์):
  | ใช้เพื่อ | ชื่อ | ตัวอย่าง |
  |---|---|---|
  | ทั่วไป | `Dto` | `UserDto` |
  | ดึง/ตอบกลับ | `RequestDto` / `ResponseDto` | `UserResponseDto` |
  | เพิ่ม / แก้ไข | `CreateDto` / `UpdateDto` | `UserCreateDto` |
  | กรอง | `FilterDto` | `UserFilterDto` |
  | แสดงตาราง | `TableDto` | `UserTableDto` |
- เก็บใน `DTOs/` แยกโฟลเดอร์ตาม Model (`DTOs/User/UserDto.cs`)
- **Flatten DTO**: ห้าม nested object → ดึงค่าขึ้นมาเป็น property ระดับบนสุด

## Controller
- Route: `[Route("api/<kebab-case-name>")]` (ตัวพิมพ์เล็ก, ใช้ `-` แทน space)
- แยก Controller ตาม domain ได้ (เช่น `ProductController`, `OrdersController`)
- Inject **Service interface** เท่านั้น
- ทุก Http method attribute ต้องมี `Name` (`[HttpGet(Name = "CallResultGetAll")]`)
- Return: ใช้ `ResponseResult.Success(...)` / `ResponseResult.NotFound<T>(...)` และคืน type `ServiceResponse<T>`
  - ยกเว้นคืนไม่ใช่ JSON → `IActionResult`
- ใส่ XML comment ทุก method (`summary`, `param`, `remarks`)
- ถ้าต้องจำกัดสิทธิ์ → `[ClaimPermission(Permission.Admin, Permission.Manager)]`
- Logging ตามความสำคัญ:
  - method ที่อ่านข้อมูล → Debug ขึ้นไป
  - method ที่เปลี่ยนแปลงข้อมูล → Information ขึ้นไป + log body request/response (ระดับ Debug ถ้า log body)

### ตัวอย่างโครง (ย่อ)
```csharp
[Route("api/call-result")]
[ApiController]
[Authorize]
public class CallResultController : ControllerBase
{
    private readonly ICallResultService _service;

    public CallResultController(ICallResultService service) { _service = service; }

    [HttpGet("{callResultId}", Name = "CallResultGetById")]
    public async Task<ServiceResponse<CallResultResponseDto?>> GetById(int callResultId, CancellationToken cancellationToken = default)
    {
        var response = await _service.GetByIdAsync(callResultId, cancellationToken);
        if (response is null)
            return ResponseResult.NotFound<CallResultResponseDto>($"Call result id {callResultId} not found");
        return ResponseResult.Success(response, $"Get call result by id {callResultId} success");
    }
}
```
> ⚠ ตัวอย่างต้นฉบับมี logger inject และ `_controllerName` ที่ใช้ก่อนประกาศ (ดู logging.md สำหรับรูปแบบที่ถูกต้อง)
