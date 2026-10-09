---
title: Backend - AutoMapper
topic: Backend
source: 00_Inbox/SIAM SMILE Web Spec 2.0.1 - Backend.md
---

# Backend: AutoMapper

**สรุปสำหรับ AI:** Map Entity ↔ DTO ด้วย **AutoMapper Profile** (`AutoMapperProfile.cs`) + inject `IMapper`. **ห้ามใช้ MappingExtension helper class** (ทำให้เกิด memory leak/ปัญหาในโปรเจกต์ใหญ่)

## Profile
```csharp
public class AutoMapperProfile : Profile
{
    public AutoMapperProfile()
    {
        CreateMap<User, UserDto>()
            .ForMember(dest => dest.FullName, opt => opt.MapFrom(src => $"{src.FirstName} {src.LastName}"));
    }
}
```

## รูปแบบการ Map
- ชื่อ property เหมือนกัน → `CreateMap<A, B>();` (map อัตโนมัติ)
- ต้องการ map กลับด้วย → `CreateMap<User, UserDto>().ReverseMap();`
- ชื่อ property ไม่ตรง → `.ForMember(dest => dest.First, opt => opt.MapFrom(src => src.FirstName))`
- ใช้ใน service:
  - Object ต่อ object: `_mapper.Map<TDest>(source)` หรือ `_mapper.Map(source, existingDest)` (update)
  - จาก query (แนะนำ, ไม่ดึงทั้ง entity): `query.ProjectTo<UserDto>(_mapper.ConfigurationProvider).ToListAsync()`

## ข้อห้าม
- ห้ามใช้ `MappingExtension` helper — ใช้ `IMapper` + Profile แทน
