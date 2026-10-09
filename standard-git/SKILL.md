---
name: standard-git
description: มาตรฐาน Git ของ SSD ใช้เมื่อต้องตั้งชื่อ branch, เขียน commit message, เลือก Git Flow (feature/release/hotfix), กำหนดเวอร์ชัน SemVer, ปล่อยเวอร์ชันด้วย release-it, ใช้ SourceTree, Pull Request หรือแก้ conflict
---

# Standard Git Convention (SSD)

ไฟล์ทั้งหมดอยู่ที่ `references/git-conventions.md` อ่านไฟล์นั้นก่อนตอบ

## กฎหลักที่ต้องยึดเสมอ
- ใช้ **Git Flow**: `master` (production), `develop` (พัฒนา), `feature/*` แตกจาก develop, `release/*` แตกจาก develop, `hotfix/*` แตกจาก master
- **Branch name:** `<type>/[optional defectNo/specNo]<description>` เช่น `feature/add-login-page`, `feature/ui1-add-login-page`, `release/1.0.0`, `hotfix/fix-login-bug`
  - Repo ที่มีหลาย project: `<type>/[projectName][defectNo/specNo]<description>` เช่น `feature/sss-add-login-page`
- **Commit message:** `<type>[(scope)]: <description>` ตาม Conventional Commits
  - type: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`
  - scope ตัวเล็กในวงเล็บ เช่น `(service)`, `(controller)`, `(deps)`, `(release)`
  - Breaking change ใส่ `!` หรือ footer `BREAKING CHANGE: <รายละเอียด>`
  - Description ไม่เกิน 50 คำ; ถ้าเป็นภาษาอังกฤษใช้ imperative
- **SemVer:** Major = เปลี่ยนแปลงที่ใช้ของเดิมไม่ได้, Minor = เพิ่ม feature, Patch = แก้ bug
- **Release:** ใช้ release-it; 1 โปรแกรมต่อ 1 repository; รันจาก branch `develop`
- **Pull Request:** ไม่ใช้ rebase ให้ใช้ PR; ก่อนทำงานทุกครั้ง fetch/pull ก่อน
- ห้าม Reset commit ที่ push ไปแล้ว ให้ใช้ Revert แทน
