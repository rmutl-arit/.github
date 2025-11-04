# มาตรฐานการพัฒนาซอฟต์แวร์ (Software Development Standards)

เอกสารนี้กำหนดหลักการ วิธีการ และข้อกำหนดขั้นต่ำ (Minimum Baseline) สำหรับการพัฒนา ดูแล และส่งมอบซอฟต์แวร์อย่างมีคุณภาพ ปลอดภัย และตรวจสอบได้

## 1) บทนำและขอบเขต
- ขอบเขต: ใช้กับทุกบริการ/ไลบรารี/CLI/แอปเว็บ/แอปโมบายขององค์กร
- วัตถุประสงค์: เพิ่มคุณภาพ ความปลอดภัย ความสม่ำเสมอ และลดความเสี่ยงในการเปลี่ยนแปลง
- คำจำกัดความสำคัญ:
  - DoR (Definition of Ready), DoD (Definition of Done)
  - SDLC (Software Development Life Cycle)
  - SLO/SLA/Error Budget
  - SemVer (Semantic Versioning)

## 2) กระบวนการพัฒนา (SDLC)

### 2.1 การวางแผนและข้อกำหนด (Requirements)
- งานต้องมี Acceptance Criteria ชัดเจนและสามารถทดสอบได้ (testable)
- ผูกงานกับ Issue/Task เสมอ และจัดลำดับความสำคัญ
- ติดตาม Traceability จากความต้องการ → การออกแบบ → โค้ด → เทส → รีลีส

### 2.2 การออกแบบ (Design)
- ใช้ ADR (Architecture Decision Record) สำหรับการตัดสินใจเชิงสถาปัตยกรรม
- ผังระบบใช้ C4 Model (ระดับ System/Container/Component)
- Threat Modeling (เช่น STRIDE) สำหรับฟีเจอร์/บริการสำคัญ

### 2.3 การพัฒนา (Implementation)
- ปฏิบัติตาม Coding Standard ของแต่ละภาษา (Linter + Formatter เป็นบังคับ)
- ห้าม Hard-code Secrets; ใช้ Secrets Manager/Env Vars/KeyVault เท่านั้น
- รองรับ i18n/l10n และ A11y (WCAG) ตามความเหมาะสม
- เขียนคอมเมนต์เท่าที่จำเป็น เน้นชื่อที่สื่อความหมายและโค้ดที่อ่านง่าย

### 2.4 การทดสอบ (Testing)
- Test Pyramid: Unit > Integration > E2E
- เกณฑ์ขั้นต่ำ: Coverage (Line/Branch) ≥ 70% สำหรับไลบรารี; ≥ 60% สำหรับบริการ
- รวม Non-functional tests ที่จำเป็น: Performance, Security, Accessibility
- จัดประเภทเทสและรันอัตโนมัติบน CI เสมอ

### 2.5 ความปลอดภัย (Security)
- นโยบาย Secure SDLC (SSDF, OWASP SAMM) เป็นฐาน
- ตรวจ SAST/Secret Scan/Dependency Scan ทุก PR และรายวันบน default branch
- ใช้มาตรฐาน OWASP ASVS (เว็บ), MASVS (โมบาย) เป็นเกณฑ์ขั้นต่ำ (ระดับ L2 แนะนำ)
- จัดทำ SBOM (SPDX/CycloneDX) สำหรับรีลีสสำคัญ; ไม่มี Critical/High vuln ที่ไม่ได้ยกเว้นอย่างมีเหตุผล

### 2.6 ประสิทธิภาพและความน่าเชื่อถือ (Reliability)
- กำหนด SLO สำคัญ (เช่น Availability, Latency P95)
- ทำ Load/Stress Test ก่อนการเปลี่ยนแปลงครั้งใหญ่
- กำหนด Error Budget และแนวทาง Freeze/Hardening เมื่อเกินงบ

### 2.7 DevOps และ CI/CD
- Pipeline ขั้นต่ำ: Lint → Build → Unit Test → Security Checks → Package → Deploy (preview/staging) → E2E/Smoke
- ใช้ Deployment Strategy ที่ลดความเสี่ยง (Blue-Green/Canary) สำหรับระบบสำคัญ
- รองรับ Rollback/Quick Revert ที่ชัดเจน

### 2.8 การสังเกตการณ์ระบบ (Observability)
- Logging แบบโครงสร้าง (JSON) + ระดับ Log ที่เหมาะสม
- Metrics ที่สำคัญ (RED/USE) และ Tracing (OpenTelemetry) สำหรับบริการแบบกระจาย
- ตั้ง Alert ที่ actionable และผูกกับ Runbook

### 2.9 การส่งมอบและเวอร์ชัน (Release & Versioning)
- ใช้ Semantic Versioning (MAJOR.MINOR.PATCH)
- ทุกรีลีสต้องมี Changelog/Release Notes และ Artifact ที่ลงลายเซ็น/Provenance (ถ้าเป็นไปได้)
- Policy สำหรับ Feature Flags และการลบฟีเจอร์ (deprecation)

### 2.10 เอกสาร (Documentation)
- เอกสารขั้นต่ำต่อโปรเจกต์: README, CONTRIBUTING, CODE_OF_CONDUCT, SECURITY, CHANGELOG, ADR/
- API/SDK ต้องมี Reference + ตัวอย่างการใช้งาน
- Runbook สำหรับปฏิบัติการและเหตุฉุกเฉิน

## 3) มาตรฐานโค้ด (Coding Standards)

### 3.1 รูปแบบและสไตล์
- บังคับใช้ Formatter (Prettier/Black/gofmt/clang-format ฯลฯ) และ Linter (ESLint/flake8/golangci-lint ฯลฯ)
- เปิดใช้ Rules ที่ป้องกัน Bug/Security Smells เป็นค่าเริ่มต้น

### 3.2 คอมมิตและข้อความคอมมิต
- ใช้ Conventional Commits:
  - ตัวอย่าง: `feat(auth): support OIDC login`
  - ประเภท: feat, fix, docs, style, refactor, perf, test, chore, build, ci
- คอมมิตเล็ก มีเหตุผล และอ้างอิง Issue/Task

### 3.3 โครงสร้างโปรเจกต์
- ชัดเจนตามแนวปฏิบัติของภาษา/เฟรมเวิร์ก
- แยกโค้ดแอป, ทดสอบ, สคริปต์, Infra-as-Code อย่างเป็นสัดส่วน

## 4) มาตรฐาน Git Workflow และ Code Review

### 4.1 Branching
- ใช้แนวทางที่ตกลงร่วมกัน (เช่น Trunk-based หรือ GitFlow แบบย่อ)
- ชื่อสาขา: `type/short-description` หรือ `issue-123/short`
- ปกป้องสาขา main: ต้องมี Review ≥ 1–2, CI ผ่าน, ห้าม force-push

### 4.2 Pull Request
- PR ขนาดเล็ก (≤ 300–400 LoC เปลี่ยนแปลงสุทธิ เป็นแนวทาง) พร้อมคำอธิบายและรูปภาพ/สคริปต์ยืนยัน
- ผูก PR กับ Issue, มี Checklist, มี Test และผลลัพธ์ CI
- Reviewer ตรวจ: ความถูกต้อง, ผลกระทบ, ความปลอดภัย, ประสิทธิภาพ, เอกสาร

## 5) คุณภาพและเกตสำคัญ (Quality Gates)
- Lint ผ่าน: ไม่มี Error ที่บล็อก
- Test ผ่าน: Coverage ตามเกณฑ์ขั้นต่ำ
- Security ผ่าน: ไม่มี Critical/High ใหม่ที่ไม่ได้ยกเว้น
- Dependency/License ตรวจสอบแล้ว (ไม่มี License ห้ามใช้)
- ได้รับการอนุมัติ Review ตามนโยบาย

## 6) ความปลอดภัยของซัพพลายเชน (Supply Chain Security)
- ใช้ Lockfile/Pin เวอร์ชันแพ็กเกจ; เปิดใช้งานเครื่องมืออัปเดตอัตโนมัติ
- ตรวจสอบที่มาของ Artifact/Container (SLSA/Provenance ถ้าเป็นไปได้)
- สแกนภาพ Container (CIS Benchmark/Best Practices)
- สร้าง/แนบ SBOM กับรีลีสสำคัญ

## 7) การเข้าถึงทรัพยากรและกรรมสิทธิ์โค้ด
- Least Privilege สำหรับสิทธิ์ทั้งหมด
- ใช้ CODEOWNERS เพื่อกำหนดเจ้าของโค้ด
- จัดการ Secrets/Keys ด้วยเครื่องมือที่เหมาะสม (ไม่เก็บในระบบ Git)

## 8) การปฏิบัติการ เหตุฉุกเฉิน และความต่อเนื่อง (Ops & DR)
- แผนสำรอง/กู้คืน (Backup/Restore) ที่ผ่านการทดสอบ
- เป้าหมาย RTO/RPO ที่สอดคล้องกับธุรกิจ
- Post-incident Review และ Action Items ที่ติดตามได้

## 9) เช็คลิสต์ขั้นต่ำ (Minimum Baseline)

### 9.1 เช็คลิสต์โปรเจกต์ใหม่
- [ ] เลือกสแต็กและมาตรฐานภาษา (Linter/Formatter) พร้อม config
- [ ] ตั้งค่า CI/CD: Build, Test, Lint, Security, Artifact
- [ ] เปิดใช้ Secret Scanning/Dependency Scan
- [ ] เพิ่มไฟล์: README, CONTRIBUTING, CODE_OF_CONDUCT, SECURITY, LICENSE, CHANGELOG, .editorconfig
- [ ] ตั้งค่า Branch protection และ CODEOWNERS
- [ ] นิยาม SLO ขั้นต่ำและ Log/Metrics/Tracing เบื้องต้น
- [ ] กำหนดเวอร์ชันเป็น SemVer และเริ่ม CHANGELOG

### 9.2 เช็คลิสต์ Pull Request
- [ ] อ้างอิง Issue/Task และบอกวัตถุประสงค์ชัดเจน
- [ ] โค้ดผ่าน Lint/Format และเพิ่ม/อัปเดต Tests
- [ ] ไม่มี Secret/ค่า Config สำคัญในโค้ด
- [ ] อัปเดตเอกสาร/Changelog (ถ้ามีผล)
- [ ] ผลกระทบด้าน Security/Performance ได้รับการพิจารณา
- [ ] Reviewer ที่ถูกต้องอนุมัติครบ และ CI ผ่านทั้งหมด

## 10) มาตรฐานและแนวทางอ้างอิง (References)
- กระบวนการ/คุณภาพ:
  - ISO/IEC/IEEE 12207 (Software lifecycle processes)
  - ISO/IEC 25010 (Software product quality model)
  - ISO/IEC/IEEE 29119 (Software testing)
  - CISQ ISO/IEC 5055 (Automated source code quality)
  - CMMI, ISO 9001 (คุณภาพองค์กร)
- ความปลอดภัย:
  - NIST SSDF (SP 800-218), OWASP SAMM, OWASP ASVS, MASVS
  - OWASP Top 10, CWE Top 25
  - SLSA (Supply-chain Levels for Software Artifacts), SBOM (SPDX/CycloneDX)
  - ISO/IEC 27001 (ISMS), SOC 2 (ถ้ามีข้อกำกับ)
- การช่วยเข้าถึงและความเป็นส่วนตัว:
  - WCAG 2.1+, GDPR/PDPA (ตามเขตอำนาจศาล)

---

ปรับเอกสารนี้ให้เหมาะกับทีมโดยการกำหนด:
- เกณฑ์ขั้นต่ำเฉพาะทีม (เช่น Coverage, SLA, Security levels)
- เทคโนโลยี/เครื่องมือมาตรฐานขององค์กร
- นโยบายยกเว้น (Exception handling) และกระบวนการอนุมัติ