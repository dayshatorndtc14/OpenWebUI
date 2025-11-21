# Meeting Transcript: Infrastructure Upgrade Discussion with Source Code Company
**Date:** November 21, 2025 (21 พฤศจิกายน 2568)  
**Project:** อพ.สธ. (Ministry of Public Health IT Project)  
**Meeting Type:** Technical Infrastructure Planning

---

## 1. Transcript (TH + Speaker Tags)

**Source Code Representative:**  
บริษัท Source Code ได้มีการเสนอการ update และ upgrade อุปกรณ์ วันนี้จะคุยเรื่อง 101F

**สนว. (Internal IT Team):**  
ที่ สนว. ดูไว้ 121G แล้ว จะไม่ได้ใช้ 101F แล้ว

**Project Manager:**  
คำถาม: โดยจะใช้งานทั้งหมดกี่ตัว?

**Technical Lead:**  
ใช้ 1 ตัว

**Project Manager:**  
มี HA (High Availability) ด้วยหรือไม่?

**Technical Lead:**  
ยังไม่มี HA

**Source Code Representative:**  
ในส่วนของ server ที่เสนอ จะมี upgrade ตัวใหม่ R260, upgrade HDD มากกว่า 16GB, และ upgrade storage ด้วย

**BC (PM Team):**  
แจ้งว่า จะมีในส่วนของ switch ที่ใช้งานได้ตามปกติ แต่เมื่อ reset password ไปแล้ว เหมือนว่าจะไม่จำค่าการตั้งค่า

**NT (Network Team):**  
แจ้งว่า อุปกรณ์ทุกตัวหมดประกันหมดแล้ว

**Source Code Representative:**  
สำหรับ Access Point:
- WIFI6 ทั้งหมด
- Wireless Controller 1 ตัว

**Technical Lead:**  
คำถาม: อุปกรณ์ที่จะจัดซื้อใหม่ ในส่วนของ server รองรับ software VMWare ESXi version ล่าสุดที่ version ใด?

**Source Code Representative:**  
Physical server รองรับ ESXi 8.0

**Unknown Speaker:**  
Power server มี single หรือไม่? **(รอการยืนยัน / Pending clarification)**

---

## 2. Meeting Notes (Structured Technical Notes)

### Agenda / วาระการประชุม
1. อุปกรณ์ที่เสนอ update/upgrade โดยบริษัท Source Code
2. การเปลี่ยนแปลงจากแบบ 101F เป็น 121G
3. ข้อกำหนดด้าน server และ storage
4. ปัญหาเกี่ยวกับ switch และการรักษาค่า configuration
5. สถานะการรับประกันอุปกรณ์
6. ข้อกำหนดด้าน wireless infrastructure
7. ความเข้ากันได้ของ VMWare ESXi version

### Attendees / ผู้เข้าร่วม
- Source Code Company Representatives (ตัวแทนบริษัท Source Code)
- สนว. (Internal IT Team)
- BC (PM Team)
- NT (Network Team)
- Technical Lead
- Project Manager

### Technical Discussion Points / ประเด็นด้านเทคนิค

#### 1. Model Selection Change
- เดิมพิจารณา: 101F
- สนว. พิจารณาเปลี่ยนเป็น: 121G
- **สถานะ:** ตัดสินใจไม่ใช้ 101F แล้ว

#### 2. Server Infrastructure
- **New Server Model:** R260
- **Storage Upgrade:** HDD > 16GB
- **Additional:** Storage upgrade included
- **Quantity:** 1 unit
- **High Availability:** ไม่มี HA ในระยะนี้

#### 3. Network Infrastructure
- **Switch Status:** ใช้งานได้ตามปกติ
- **Issue Identified:** หลังจาก reset password แล้ว configuration ไม่ถูกบันทึก (ไม่จำค่า)
- **Warranty Status:** อุปกรณ์ทุกตัวหมดประกันแล้ว

#### 4. Wireless Infrastructure
- **Access Points:** WIFI6 standard (ทั้งหมด)
- **Wireless Controller:** 1 unit

#### 5. Virtualization Platform
- **Software:** VMWare ESXi
- **Supported Version:** ESXi 8.0 (รองรับโดย physical server ที่เสนอ)

#### 6. Power Configuration
- **Status:** รอการยืนยันเรื่อง power redundancy (single vs redundant)

---

## 3. Technical Decisions / Design Decisions

### Confirmed Decisions
1. **เปลี่ยนจาก 101F ไปใช้ 121G** - ตามข้อเสนอของ สนว.
2. **Server Platform:** R260 พร้อม storage และ HDD upgrade
3. **Wireless Standard:** WIFI6 เป็นมาตรฐาน
4. **Virtualization:** VMWare ESXi 8.0 compatibility required
5. **Deployment Mode:** Single server (no HA initially)

### Pending Decisions
1. **Power Configuration:** ต้องยืนยันว่าใช้ single power supply หรือ redundant power
2. **Switch Configuration Persistence:** ต้องแก้ไขปัญหาการไม่บันทึก configuration หลัง password reset

---

## 4. Action Items

| Action Item | Assignee | Priority | Deadline | Status |
|-------------|----------|----------|----------|--------|
| ยืนยัน power configuration (single/redundant) สำหรับ server R260 | Source Code Team | High | รอการยืนยัน | Pending |
| สอบถามและแก้ไขปัญหา switch ที่ไม่บันทึก configuration หลัง password reset | BC (PM Team) / NT (Network Team) | High | รอกำหนด | Open |
| จัดทำเอกสารรายละเอียดทางเทคนิคของ R260 server และการรองรับ ESXi 8.0 | Source Code Team | Medium | รอกำหนด | Open |
| ตรวจสอบและวางแผนการต่ออายุประกันอุปกรณ์ที่หมดประกัน | NT (Network Team) | High | รอกำหนด | Open |
| ระบุรายละเอียด WIFI6 AP และ Wireless Controller specifications | Source Code Team | Medium | รอกำหนด | Open |
| ประเมินความจำเป็นของ HA (High Availability) ในอนาคต | Technical Lead / Project Manager | Low | รอกำหนด | Open |

---

## 5. Risks / Issues / Technical Concerns

### Critical Issues
1. **อุปกรณ์หมดประกัน (Warranty Expired)**
   - **Impact:** ความเสี่ยงสูงในกรณีอุปกรณ์เสีย ไม่มีการรับประกันหรือซ่อม
   - **Mitigation:** ควรพิจารณาการต่ออายุประกันหรือการจัดซื้ออุปกรณ์ใหม่

2. **Switch Configuration Loss After Password Reset**
   - **Impact:** อาจทำให้เกิดการหยุดชะงักของระบบเครือข่าย
   - **Mitigation:** ต้องระบุสาเหตุและแก้ไขก่อนการ deploy ระบบใหม่

### Medium Risks
3. **No High Availability (Single Server)**
   - **Impact:** Single point of failure สำหรับระบบทั้งหมด
   - **Mitigation:** ควรวางแผน HA solution ในระยะถัดไป

4. **Power Supply Configuration Unclear**
   - **Impact:** อาจไม่มี redundancy ในกรณี power failure
   - **Mitigation:** ต้องยืนยันและพิจารณา redundant power supply

### Technical Debt
5. **Legacy Model (101F) Replacement**
   - **Impact:** ต้องมีการวางแผน migration และ compatibility testing
   - **Mitigation:** จัดทำแผนการ migration และ testing ก่อน deployment

---

## 6. Dependencies / บล็อกเกอร์ / สิ่งที่ต้องรอ

1. **รอการยืนยัน:** Power configuration details สำหรับ server R260
2. **รอการแก้ไข:** Switch configuration persistence issue
3. **รอเอกสาร:** Technical specifications จาก Source Code สำหรับ R260, WIFI6 AP, และ Wireless Controller
4. **รอการตัดสินใจ:** แผนการต่ออายุประกันอุปกรณ์
5. **รอการประเมิน:** ความจำเป็นและ timeline สำหรับการเพิ่ม HA

---

## 7. Executive Summary (TH)

### สรุปสำหรับผู้บริหาร

ในการประชุมวันที่ 21 พฤศจิกายน 2568 ระหว่างทีมโครงการ อพ.สธ. และบริษัท Source Code ได้มีการหารือเกี่ยวกับการ upgrade โครงสร้างพื้นฐานด้าน IT ดังนี้:

**ความคืบหน้าหลัก:**
- เปลี่ยนแปลงจากแบบ 101F เป็น 121G ตามคำแนะนำของ สนว.
- เสนอ server model R260 พร้อม storage และ HDD upgrade รองรับ VMWare ESXi 8.0
- กำหนดใช้ WIFI6 standard สำหรับ wireless infrastructure พร้อม Wireless Controller 1 ตัว
- ระบบจะใช้ single server (ยังไม่มี HA)

**ปัญหาและความเสี่ยงสำคัญ:**
1. **อุปกรณ์ทั้งหมดหมดประกันแล้ว** - ต้องการการดำเนินการเร่งด่วนในการต่ออายุประกันหรือจัดซื้ออุปกรณ์ใหม่
2. **Switch มีปัญหาไม่บันทึก configuration หลัง password reset** - ต้องแก้ไขก่อนการ deploy
3. **ไม่มี High Availability** - มีความเสี่ยง single point of failure

**สิ่งที่ต้องดำเนินการต่อ:**
- ยืนยัน power configuration สำหรับ server
- แก้ไขปัญหา switch configuration
- จัดทำแผนการต่ออายุประกันอุปกรณ์
- ประเมินความจำเป็นของ HA solution

**ผลกระทบต่อ Timeline:**
- การแก้ไขปัญหา switch และการต่ออายุประกันอาจส่งผลต่อ timeline การ deploy
- ควรมีการติดตามและแก้ไขปัญหาที่ระบุเป็นลำดับแรก

---

## 8. Executive Summary (EN)

### Executive Summary

On November 21, 2025, a meeting was held between the Ministry of Public Health IT Project team and Source Code Company to discuss IT infrastructure upgrades:

**Key Progress:**
- Changed equipment model from 101F to 121G based on internal IT team (สนว.) recommendation
- Proposed R260 server model with storage and HDD upgrades, supporting VMWare ESXi 8.0
- Adopted WIFI6 standard for wireless infrastructure with 1 Wireless Controller unit
- System will use single server deployment (no HA initially)

**Critical Issues and Risks:**
1. **All equipment warranties have expired** - Urgent action needed for warranty renewal or new equipment procurement
2. **Switch configuration not persisting after password reset** - Must be resolved before deployment
3. **No High Availability** - Single point of failure risk exists

**Next Actions Required:**
- Confirm power configuration for server
- Resolve switch configuration persistence issue
- Develop warranty renewal plan
- Assess HA solution requirements

**Timeline Impact:**
- Switch configuration issues and warranty renewal may impact deployment timeline
- Priority should be given to resolving identified critical issues

**Technical Decisions Summary:**
- VMWare ESXi 8.0 compatibility confirmed
- WIFI6 standard adopted
- Single server approach initially (HA to be considered in future phases)

---

## Document Metadata

- **Document Type:** Meeting Transcript & Technical Notes
- **Created:** November 21, 2025
- **Format Version:** 1.0
- **Language:** Bilingual (Thai/English)
- **Classification:** Internal - Project Documentation
- **Related Project:** อพ.สธ. Ministry of Public Health IT Infrastructure Upgrade

---

*This document was generated following the Bilingual Meeting Transcription, Notes, and Summary format for technical/engineering meetings.*
