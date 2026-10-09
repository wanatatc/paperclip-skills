---
title: Git - Conventions, Commit, Versioning & Release
topic: Git (Backend + Frontend)
source: 00_Inbox/SIAM SMILE Web Spec 2.0.1 - Backend.md
---

# Git: Conventions, Commit, Versioning & Release

**สรุปสำหรับ AI:** ใช้ **Git Flow** (master / develop / feature / release / hotfix), branch และ commit ตาม Conventional Commits, เวอร์ชันตาม SemVer, ปล่อยเวอร์ชันด้วย release-it. ใช้ SourceTree เป็น GUI ได้. **ไม่ rebase → ใช้ Pull Request**

## Git Flow
| Branch | ใช้ | แตกจาก | merge กลับไป |
|---|---|---|---|
| `master` | production เท่านั้น | - | - |
| `develop` | พัฒนาปกติ | master | - |
| `feature/<desc>` | ฟีเจอร์ใหม่ | develop | develop |
| `release/<version>` | เตรียมปล่อย | develop | master + develop |
| `hotfix/<desc>` | แก้ production ด่วน | master | master + develop |

## Workflow
1. สร้าง repo บน GitHub ชื่อเดียวกับโปรเจกต์ → clone
2. feature: branch จาก develop → commit/push → Pull Request เข้า develop → review → merge → ลบ branch
3. release: Dev Lead ตัดจาก develop ด้วย release-it → Tech Lead merge เข้า master และ develop หลัง deploy สำเร็จ
4. hotfix: branch จาก master → commit/push → PR เข้า master (แล้วนำกลับ develop)

## Branch naming
`<type>/[optional defectNo/specNo]<description>`
- `feature/add-login-page`, `feature/ui1-add-login-page`, `release/1.0.0`, `hotfix/fix-login-bug`
- Repo ที่มีหลาย project: `<type>/[projectName][defectNo/specNo]<description>` เช่น `feature/sss-add-login-page`, `release/sss-1.0.0`

## Commit message (Conventional Commits)
รูปแบบ: `<type>[(scope)]: <description>` + body/footer (เว้นบรรทัดว่าง)

**type:** `feat` (feature ใหม่), `fix` (แก้ bug), `docs`, `style` (ไม่กระทบการทำงาน เช่นสี/ตำแหน่ง), `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`

**scope** (ตัวเล็ก ในวงเล็บ): `merge`, `release`, `config`, `deps`, `service`, `controller`, `model`, `redux`, `api` ฯลฯ

**Breaking change:** ใส่ `!` หลัง type/scope หรือ footer `BREAKING CHANGE: <รายละเอียด>` (เช่น เพิ่ม config ใหม่, เปลี่ยน API, เปลี่ยนวิธีใช้ config)

**Description:** ≤ 50 คำ, ไทยหรือ English (English ใช้ imperative ไม่ใส่ประธาน)

ตัวอย่าง:
- `feat(service): add order service`
- `fix: fix bug in login page`
- `refactor: change order status to enum`
- `chore(release): v1.0.1`
- `chore(deps): update restsharp version to 109.0.5`
- `style: change color of button`

## Versioning (SemVer)
- **Major** — เปลี่ยนแปลงที่ใช้ของเดิมไม่ได้
- **Minor** — เพิ่ม feature ใหม่
- **Patch** — แก้ bug

## Release-it
- ติดตั้ง: `npm install -g @release-it/conventional-changelog@8.0.1 release-it@17.3.0`
- เงื่อนไข: **1 โปรแกรม ต่อ 1 repository** (มากกว่า 1 → แจ้งพี่มาร์คให้แยก)
- เพิ่มในโปรเจกต์: `package.json` (frontend ให้ version ตรงปัจจุบัน / backend เพิ่มไฟล์ใหม่), `.release-it.json`, commit `build: add release-it`
- ใช้งาน: switch ไป `develop` → `release-it` → เลือก major/minor/patch → Commit ✔ Tag ✔ Push ✔ Create GitHub release ✔

## SourceTree (GUI)
- ต้องมี Git + .NET Framework 4.6.1+
- Setup: Preferences → ชื่อภาษาอังกฤษ + อีเมลเดียวกับ GitHub, SSH key = No
- Authentication: Tools → Options → Authentication → GitHub / HTTPS / OAuth
- Clone: `+` → Remote → Show Organization Repos → SiamSmileDev → Clone
- ต้อง **Git Flow → Initialize Repository** หลัง clone
- Commit แยกหลาย commit ได้โดย Stage ทีละกลุ่มไฟล์
- Reset: Soft / Mixed / Hard — **อย่า Reset commit ที่ push แล้ว** ใช้ Revert แทน
- ก่อนเริ่มงานทุกครั้ง: Fetch → Pull
- Conflict: `Merge develop into current branch` → Resolve (Mine / Theirs / แก้เอง) → Mark Resolved → Commit
