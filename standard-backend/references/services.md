---
title: Backend - Services
topic: Backend
source: 00_Inbox/SIAM SMILE Web Spec 2.0.1 - Backend.md
---

# Backend: Services

**สรุปสำหรับ AI:** Service = business logic ของแต่ละ domain. **ต้องมี Interface คู่กันเสมอ** (สำหรับ DI และ unit test). Inject `DbContext`, `IMapper`, `ILoginDetailServices` (เฉพาะที่ต้องใช้ user), logger

## กฎ
- ทุก Service มี `I<Name>Service` interface (จะอยู่ไฟล์เดียวกันหรือแยกก็ได้)
- โฟลเดอร์: ตามดุลพินิจ Team Lead; ถ้ามี interface ให้อยู่โฟลเดอร์เดียวกับ service
  - เช่น `Services/CallResult/ICallResultService.cs`, `CallResultService.cs`
- `ServiceBase` = class พื้นฐานสำหรับเขียน service
- ใช้ `AsNoTracking()` กับ query อ่านอย่างเดียว
- Filter ด้วย method แยก (`FilterByDto(query, filter)`) และ paginate ด้วย extension `Paginate` / `PaginationResult`
- Project ไป DTO ด้วย `ProjectTo<TDto>(_mapper.ConfigurationProvider)` ไม่ select เอง
- ผู้ใช้ปัจจุบัน: `_login.GetClaim()` → `UserId`
- ไม่พบข้อมูล → `throw new NotFoundException(...)`
- Log ที่ action สำคัญ + เขียน log `Information` หลัง `SaveChangesAsync` สำเร็จ

## Interface สรุป (รูปแบบมาตรฐาน)
```csharp
public interface ICallResultService
{
    Task<List<CallResultTableDto>> GetAllAsync(PaginationDto paginationDto, CallResultFilterDto? filter = null, CancellationToken cancellationToken = default);
    Task<PaginationResultDto> GetAllPaginationAsync(PaginationDto paginationDto, CallResultFilterDto? filter = null, CancellationToken cancellationToken = default);
    Task<CallResultResponseDto?> GetByIdAsync(int id, CancellationToken cancellationToken = default);
    Task<CallResultResponseDto> CreateAsync(CallResultRequestDto request, CancellationToken cancellationToken = default);
    Task<CallResultResponseDto?> UpdateAsync(int id, CallResultRequestDto request, CancellationToken cancellationToken = default);
    Task<CallResultResponseDto?> DeleteAsync(int id, CancellationToken cancellationToken = default);
}
```

## Constructor (รูปแบบมาตรฐาน)
```csharp
public CallResultService(AppDbContext dbContext, IMapper mapper, ILoginDetailServices login, Serilog.ILogger? logger = null)
{
    _dbContext = dbContext;
    _mapper = mapper;
    _login = login;
    _logger = (logger ?? Serilog.Log.Logger).ForContext("ServiceName", _serviceName);
}
private const string _serviceName = nameof(CallResultService);
```

## Create / Update pattern
```csharp
var userId = _login.GetClaim().UserId;
var now = DateTime.Now;

var entity = _mapper.Map<CallResult>(request);
entity.CreatedByUserId = userId; entity.CreatedDate = now;
entity.UpdatedByUserId = userId; entity.UpdatedDate = now;
_dbContext.Add(entity);
await _dbContext.SaveChangesAsync(cancellationToken);
return _mapper.Map<CallResultResponseDto>(entity);
```
Update: `FirstOrDefaultAsync` → ไม่เจอ throw `NotFoundException` → `_mapper.Map(request, entity)` → set `UpdatedByUserId/UpdatedDate` → save

> ⚠ ตัวอย่างต้นฉบับมีจุดที่ยังไม่สมบูรณ์ (เช่น `CheckCallResult(request, result1, result2)` ที่ตัวแปรไม่ได้ประกาศ, และ `GetAllPaginationAsync` คืน `List<...>` ขณะที่ interface ระบุ `PaginationResultDto`) — ใช้เป็นรูปแบบ ไม่ใช่โค้ดคอมไพล์ได้
