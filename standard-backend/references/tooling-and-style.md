---
title: Backend - Tooling & C# Style
topic: Backend
source: 00_Inbox/SIAM SMILE Web Spec 2.0.1 - Backend.md
---

# Backend: Tooling & C# Style

**สรุปสำหรับ AI:** ASP.NET Core + C#. ใช้ Visual Studio 2022. Base = Microsoft C# Naming Guidelines + กฎ SSD ด้านล่าง

## IDE
- Visual Studio 2022
- Extensions: EF Core Power Tools (reverse DB), NUnit Test Generator
- VS Options: Indent 4 spaces, Tab 4, Insert spaces = True. Naming: Interface begin with `I`, private field begin with `_` (camelCase)

## Naming (SSD)
- **PascalCase:** class, method, enum, public field/property, namespace, file, folder
- **camelCase:** local variable, parameter
- **`_camelCase`:** private / protected / internal fields & properties
- **Interface:** prefix `I` (`ICallResultService`)
- **คำย่อ:**
  - หลายคำ → ตัวใหญ่ทั้งหมด: `DTO`, `IT`, `SMI`, `API`, `HTML`
  - 1 คำ → ตัวแรกใหญ่: `Id`, `Cust`, `Doc`, `Temp`
  - พยางค์แรกของ camelCase → ตัวเล็กทั้งหมด: `html`, `xml`
  - พยางค์ที่ 2+ → ตัวใหญ่: `docHTML`, `_docXML`
  - ยกเว้นคำย่อ 1 คำที่พยางค์ 2: camelCase: `docId`, `custId`
- Modifier (const, static, readonly) ไม่บังคับกฎชื่อ
- ชื่อไฟล์ = ชื่อ class หลัก, 1 ไฟล์ = 1 class หลัก

## Layout / ordering
- `using` อยู่บนสุด ก่อน namespace, เรียง a-z, `System` ขึ้นก่อนเสมอ
- Modifier order: `public protected internal private new abstract virtual override sealed static readonly extern unsafe volatile async`
- Class members: (1) nested class/enum/delegate/event (2) static/const/readonly field (3) field & property (4) constructor & finalizer (5) method
- ในแต่ละกลุ่ม: public → internal → protected internal → protected → private
- 1 statement / 1 บรรทัด, 1 declaration / 1 บรรทัด, เว้นบรรทัดระหว่าง method/property
- `{` ขึ้นบรรทัดใหม่ (Allman style)

## Comments
- คอมเมนต์แยกบรรทัด ห้ามต่อท้ายคำสั่ง
- ภาษาอังกฤษ: ตัวแรกพิมพ์ใหญ่, จบด้วยจุด, เว้น 1 ช่องหลัง `//`
- ห้ามใช้ `/* */` ครอบคอมเมนต์ซ้ำ
- Public member → XML doc comment (`/// <summary>`)
