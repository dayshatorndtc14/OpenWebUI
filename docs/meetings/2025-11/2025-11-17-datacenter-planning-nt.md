# Data Center Planning Meeting with NT
## ประชุม Data Center ร่วมกับ NT

**Date / วันที่:** November 17, 2025 (17 พฤศจิกายน 2568)  
**Meeting Type / ประเภท:** Infrastructure Planning Session / การวางแผนโครงสร้างพื้นฐาน  
**Organization / หน่วยงาน:** NT Company

---

## 📋 Transcript (Meeting Discussion Points)

### **Infrastructure Requirements Discussion**

**Lead Architect (พี่เกษม - NT):**
เราได้รวบรวมข้อมูลจากการสำรวจเบื้องต้น พบว่ามีทั้งหมด 36 สถานที่ มีเครื่อง client 3,089 เครื่อง physical server 102 เครื่อง มี 217 units และ VM 233 ตัว รวมระบบทั้งหมด 119 ระบบ

สำหรับการแบ่งขนาด site เราแนะนำดังนี้:
- Small sites (1-20 เครื่อง): ใช้ 1 SD-WAN
- Medium sites (20-90 เครื่อง): ใช้ 1 SD-WAN + 1 MPLS
- Campus sites (500+ เครื่อง): ใช้ 2 SD-WAN + 2 MPLS

**Technical Team:**
สำหรับโครงสร้างปัจจุบันที่ทิพย์พิมาน มี gateway หลายตัวต่อ site ควรจะจัดการอย่างไร?

**Lead Architect (พี่เกษม - NT):**
เราจะแบ่ง network zones ออกเป็น 5 zones:
- Internet zone
- Server zone
- Campus zone
- User zone
- Management zone

สำหรับ Data Center ให้เน้นทำก่อน แล้วค่อยขยายไป branches ทีหลัง

### **Security and Implementation Strategy**

**Lead Architect (พี่เกษม - NT):**
สำหรับความปลอดภัย ให้จัดลำดับความสำคัญตามระบบ:
1. Spine (แกนหลัก)
2. Core Firewall
3. Top-of-Rack Firewall
4. Server level

และแบ่ง Service grouping ตามความสำคัญ:
- High priority zone
- Medium priority zone
- General zone

สำหรับ static port mapping ให้ใช้กับระบบที่ critical เท่านั้น

### **Day 1 Implementation Concerns**

**Project Manager:**
สำหรับ Day 1 เราควรทำอย่างไรให้ผู้ใช้ไม่ต่อต้าน?

**Lead Architect (พี่เกษม - NT):**
ไม่แนะนำให้ใช้ AD integration ใน Day 1 เพราะจะทำให้ผู้ใช้ต่อต้าน ให้ใช้ centralized AD สำหรับ user management แทน และเชื่อมต่อผ่าน API

สำหรับ ZTNA ไม่แนะนำให้ทำเลย เพราะ:
- Vendor dependency สูง
- ต้องการ staff ที่มีความสามารถเฉพาะทาง
- Agent vs agentless มีข้อจำกัดทั้งคู่

### **TRD Security Recommendation**

**Security Team:**
TRD แนะนำให้แยก dev/UAT/production ด้วย VLAN เพื่อความปลอดภัย

**Lead Architect (พี่เกษม - NT):**
ถูกต้อง นี่เป็น best practice ที่ควรทำ

### **Data Verification Issue**

**Technical Team:**
เราพบความคลาดเคลื่อนของข้อมูล site หนึ่งรายงานว่ามี 2 เครื่อง แต่จริงๆ มีมากกว่า 100 เครื่อง

**Lead Architect (พี่เกษม - NT):**
ต้อง verify ข้อมูลจำนวนเครื่องจริงในแต่ละ site ให้แน่ชัด เพราะจะส่งผลต่อการ sizing และงบประมาณ

### **Migration Timeline Concerns**

**คุณดอน:**
มีความกังวลเรื่อง timeline การ migrate ว่าจะใช้เวลานานเกินไปหรือไม่?

**Lead Architect (พี่เกษม - NT):**
เราจะทำแบบ phased approach ตาม Phase 0-4 ที่วางแผนไว้ เพื่อลดความเสี่ยง

---

## 📝 Meeting Notes (Structured Technical Notes)

### **Attendees / ผู้เข้าร่วม**

- **พี่เกษม** (NT) - Lead Architect / Consultant
- **พี่นิด** - Technical Team
- **พี่นะ** - Technical Team
- **คุณดอน** - Project Stakeholder
- **DTC Team** - Implementation Team

---

### **Agenda / วาระการประชุม**

1. Review current infrastructure data and statistics
2. Discuss network architecture for different site sizes
3. Security strategy and zoning design
4. Day 1 implementation approach
5. ZTNA and AD integration considerations
6. Phased implementation roadmap (Phase 0-4)
7. Action items and next steps

---

### **Technical Discussion Points / ประเด็นด้านเทคนิค**

#### **1. Current Infrastructure Data / ข้อมูลโครงสร้างปัจจุบัน**

**Data Collected:**
- **Total Sites:** 36 locations
- **Client Machines:** 3,089 units
- **Physical Servers:** 102 units
- **Total Units:** 217
- **Virtual Machines:** 233 VMs
- **Total Systems:** 119 systems

**Site Classification by Size:**
- **Small Sites:** 1-20 machines → 1 SD-WAN
- **Medium Sites:** 20-90 machines → 1 SD-WAN + 1 MPLS
- **Large Sites:** 90-500 machines → (configuration pending)
- **Campus Sites:** 500+ machines → 2 SD-WAN + 2 MPLS

**Current Setup (ทิพย์พิมาน):**
- Multiple gateways per site
- Requires consolidation and standardization

#### **2. Network Architecture Design / สถาปัตยกรรมเครือข่าย**

**Network Zones (5 Zones):**
1. **Internet Zone** - External connectivity
2. **Server Zone** - Application and database servers
3. **Campus Zone** - Campus-wide services
4. **User Zone** - End-user workstations
5. **Management Zone** - Infrastructure management

**Technology Stack:**
- **SD-WAN:** Software-defined WAN for flexible connectivity
- **MPLS:** Multi-Protocol Label Switching for guaranteed bandwidth
- **Hybrid Approach:** Combination based on site size and criticality

#### **3. Security Architecture / สถาปัตยกรรมด้านความปลอดภัย**

**Security Layer Priority (Top to Bottom):**
1. **Spine** - Core backbone infrastructure
2. **Core Firewall** - Data center perimeter
3. **Top-of-Rack Firewall** - Server rack level
4. **Server** - Host-based security

**Service Grouping by Priority:**
- **High Priority Zone** - Critical business systems
- **Medium Priority Zone** - Important but non-critical systems
- **General Zone** - Standard office applications

**Port Mapping Strategy:**
- **Static Port Mapping:** Only for critical systems
- **Dynamic Allocation:** For general services
- **TRD Recommendation:** Separate dev/UAT/production by VLAN

#### **4. Authentication and Access Management**

**AD Integration Strategy:**
- **Not recommended for Day 1** - To minimize user resistance
- **Centralized AD** - For user management
- **API Integration** - For connecting systems
- **ThaID Usage** - Pending clarification on authentication scope

**ZTNA (Zero Trust Network Access) Decision:**
- **Recommendation:** Do NOT implement
- **Reasons:**
  - High vendor dependency
  - Requires specialized staff capabilities
  - Agent vs agentless trade-offs create limitations
  - Organizational readiness concerns

---

### **Architecture / Design Decisions**

| Decision Area | Approach Selected | Rationale |
|---------------|-------------------|-----------|
| **Implementation Priority** | Data Center first, then branches | Reduces complexity, establishes core foundation |
| **Network Zones** | 5-zone architecture | Proper segmentation for security and management |
| **Site Connectivity** | Hybrid SD-WAN + MPLS based on size | Balances cost, performance, and reliability |
| **Security Model** | Layered defense (Spine → Server) | Defense in depth approach |
| **AD Integration** | Centralized AD with API | Avoids Day 1 user resistance |
| **ZTNA** | Not implementing | Vendor lock-in and capability concerns |
| **Environment Separation** | VLAN-based (dev/UAT/prod) | Security best practice per TRD |

---

### **Decisions Made / ข้อสรุป**

1. ✅ **Focus on Data Center infrastructure before branch expansion**
2. ✅ **Implement 5-zone network architecture**
3. ✅ **Use hybrid SD-WAN/MPLS approach based on site size**
4. ✅ **Do NOT implement ZTNA due to dependency concerns**
5. ✅ **Do NOT integrate AD on Day 1 - use centralized AD with API instead**
6. ✅ **Implement layered security from Spine to Server level**
7. ✅ **Separate environments by VLAN (dev/UAT/production)**
8. ✅ **Follow Phase 0-4 POC approach for implementation**
9. ⚠️ **ThaID authentication scope** - Pending clarification
10. ⚠️ **Actual machine count per site** - Requires verification

---

## 🎯 Action Items

| # | Action Item | Assignee | Deadline | Priority | Status |
|---|-------------|----------|----------|----------|--------|
| 1 | Verify actual machine count per site (address discrepancy: reported 2 vs actual 100+) | DTC Team | Week 1 | 🔴 High | Pending |
| 2 | Define Day 1 scope and prioritize purchases | Project Team | Week 1 | 🔴 High | Pending |
| 3 | Finalize Data Center architecture diagram | พี่เกษม (NT) | Week 2 | 🔴 High | Pending |
| 4 | Classify systems by priority for security implementation (High/Medium/General zones) | Security Team | Week 2 | 🟠 Medium | Pending |
| 5 | Clarify ThaID usage scope and authentication requirements | DTC Team | Week 2 | 🟠 Medium | Pending |
| 6 | Address migration timeline concerns raised by คุณดอน | Project Manager | Week 1 | 🟠 Medium | Pending |
| 7 | Prepare Phase 0 PoC plan (select max 3 vendors) | Technical Team | Week 3 | 🟡 Normal | Pending |
| 8 | Document static port mapping requirements for critical systems | Technical Team | Week 2 | 🟡 Normal | Pending |

---

## ⚠️ Risks / Issues / Technical Concerns

### **Critical Issues**

1. **❌ Data Accuracy Problem**
   - **Issue:** Significant discrepancy in machine count reporting (reported 2, actual 100+)
   - **Impact:** Affects infrastructure sizing, cost estimates, and capacity planning
   - **Mitigation:** Immediate audit of all site machine counts required

2. **❌ Migration Timeline Concerns (คุณดอน)**
   - **Issue:** Uncertainty about migration duration and business impact
   - **Impact:** Potential delays, budget overruns
   - **Mitigation:** Detailed Phase 0-4 timeline with clear milestones

### **Technical Risks**

3. **⚠️ User Resistance on Day 1**
   - **Risk:** Changes to authentication or access could face user pushback
   - **Mitigation:** Defer AD integration, minimize Day 1 changes, focus on backend improvements

4. **⚠️ Vendor Dependency (ZTNA)**
   - **Risk:** If implemented, would create vendor lock-in
   - **Decision:** Not implementing ZTNA to avoid this risk

5. **⚠️ Staff Capability Gap**
   - **Risk:** Specialized skills required for advanced security features
   - **Mitigation:** Focus on technologies within current team capabilities

### **Pending Clarifications**

6. **❓ ThaID Authentication Scope**
   - **Status:** Unclear how ThaID will be integrated
   - **Required:** Define authentication boundaries and use cases

7. **❓ Day 1 Purchase Priority**
   - **Status:** Not yet defined
   - **Required:** Budget allocation and procurement timeline

---

## 🔗 Dependencies / Blockers

| Dependency | Impact | Owner | Status |
|------------|--------|-------|--------|
| Complete site audit (machine counts) | Blocks accurate sizing and budgeting | DTC Team | 🔴 Blocking |
| Day 1 scope definition | Blocks procurement planning | Project Team | 🔴 Blocking |
| ThaID clarification | Blocks authentication design | DTC Team | 🟠 At Risk |
| Architecture diagram approval | Blocks detailed design phase | NT / Project Team | 🟡 In Progress |

---

## 📊 Implementation Roadmap (Phase 0-4 POC)

### **Phase 0: Preparation**
**Duration:** 2-3 months  
**Objectives:**
- Study product capabilities from vendors
- Conduct PoC with maximum 3 vendors
- Select 1-2 vendors for trial implementation

**Activities:**
- Vendor evaluation and RFP process
- Technical requirements documentation
- Test environment setup
- Proof of Concept execution

---

### **Phase 1: Trial**
**Duration:** 6-12 months  
**Objectives:**
- Test selected solution with sample group
- Validate technical approach
- Identify issues and optimization opportunities

**Scope:**
- Deploy to ≤15% of total users
- Selected pilot sites representing different sizes
- Monitor performance and gather feedback

**Success Criteria:**
- System stability > 99%
- User satisfaction feedback positive
- No critical security incidents

---

### **Phase 2: Adaptation**
**Duration:** 3-6 months  
**Objectives:**
- Refine rules and configurations based on Phase 1 learnings
- Conduct training for operations team
- Optimize staffing and procedures

**Activities:**
- Rule optimization
- Security policy refinement
- Training program execution
- Documentation updates
- Staffing adjustments

---

### **Phase 3: Expansion**
**Duration:** 6-9 months  
**Objectives:**
- Scale deployment to significant user base
- Validate production readiness
- Establish operational excellence

**Scope:**
- Scale to 20-50% of users
- Include diverse site types
- Full operational procedures in place

---

### **Phase 4: Full Coverage**
**Duration:** 6-12 months  
**Objectives:**
- Complete rollout to all users
- Achieve 100% coverage
- Transition to BAU (Business As Usual)

**Scope:**
- Scale to 100% of users and sites
- Complete migration of all systems
- Decommission legacy infrastructure

**Success Criteria:**
- All sites migrated successfully
- Operations team fully trained and autonomous
- Service levels meet or exceed targets

---

## 📋 Executive Summary (Thai)

### สรุปสำหรับผู้บริหาร

**ภาพรวมการประชุม:**  
การประชุมวางแผน Data Center ร่วมกับ NT เมื่อวันที่ 17 พฤศจิกายน 2568 เพื่อกำหนดทิศทางการพัฒนาโครงสร้างพื้นฐานด้าน IT และความปลอดภัย

**ข้อมูลโครงสร้างปัจจุบัน:**
- มี 36 สถานที่ ประกอบด้วยเครื่อง client 3,089 เครื่อง server 102 เครื่อง VM 233 ตัว และระบบทั้งหมด 119 ระบบ
- พบความคลาดเคลื่อนของข้อมูลจำนวนเครื่องในบาง site (รายงาน 2 เครื่อง แต่จริงมากกว่า 100 เครื่อง)

**สถาปัตยกรรมที่เสนอ:**
1. **Network Zones:** แบ่งเป็น 5 zones (Internet, Server, Campus, User, Management)
2. **Site Connectivity:** ใช้ SD-WAN และ MPLS แบบ hybrid ตามขนาด site
3. **Security Layers:** ใช้การป้องกันแบบหลายชั้น จาก Spine ถึง Server level
4. **Environment Separation:** แยก dev/UAT/production ด้วย VLAN

**ข้อเสนอแนะสำคัญจาก NT:**
- ✅ เริ่มที่ Data Center ก่อน แล้วค่อยขยายไป branches
- ✅ ใช้ Centralized AD พร้อม API integration แทนการ integrate AD โดยตรงใน Day 1
- ❌ ไม่แนะนำให้ทำ ZTNA เนื่องจาก vendor dependency และข้อจำกัดด้าน staff
- ✅ ใช้ static port mapping เฉพาะระบบที่ critical
- ✅ แบ่งระบบตามความสำคัญเป็น High/Medium/General priority zones

**แผนการดำเนินงาน (Phase 0-4):**
- **Phase 0:** เตรียมการและ PoC (2-3 เดือน) - เลือก vendor สูงสุด 3 ราย
- **Phase 1:** Trial กับกลุ่มตัวอย่าง ≤15% (6-12 เดือน)
- **Phase 2:** ปรับปรุงและฝึกอบรม (3-6 เดือน)
- **Phase 3:** ขยายผลไป 20-50% (6-9 เดือน)
- **Phase 4:** ครอบคลุม 100% (6-12 เดือน)

**ประเด็นที่ต้องดำเนินการทันที:**
1. 🔴 **Critical:** ตรวจสอบจำนวนเครื่องจริงในแต่ละ site เพื่อความถูกต้องของการ sizing
2. 🔴 **Critical:** กำหนดขอบเขต Day 1 และจัดลำดับความสำคัญการจัดซื้อ
3. 🟠 **สำคัญ:** ชี้แจงขอบเขตการใช้งาน ThaID สำหรับ authentication

**ความเสี่ยง:**
- ข้อมูลไม่ถูกต้องอาจส่งผลต่อการประมาณการงบประมาณและ capacity planning
- ความกังวลเรื่อง timeline การ migrate จากคุณดอน ต้องมีแผนที่ชัดเจน
- ความต้านทานจากผู้ใช้ใน Day 1 หากมีการเปลี่ยนแปลงมากเกินไป

**ผลกระทบต่อ Timeline:**
- โครงการใช้เวลารวมประมาณ 2-4 ปี สำหรับ Phase 0-4 ทั้งหมด
- Phase 0-1 (8-15 เดือน) เป็นช่วงสำคัญที่สุดในการทดสอบและปรับปรุง

**ข้อสรุปสำคัญ:**
- การวางแผนเน้นความระมัดระวังและลดความเสี่ยง ด้วยการทำแบบ phased approach
- ไม่รีบร้อนใช้เทคโนโลยีที่ซับซ้อนเกินไป (เช่น ZTNA) หากทีมยังไม่พร้อม
- ให้ความสำคัญกับ Data Center ก่อน เพื่อสร้างฐานที่มั่นคง
- การตรวจสอบข้อมูลให้ถูกต้องเป็นสิ่งสำคัญที่สุดก่อนเริ่มโครงการ

---

## 📋 Executive Summary (English)

### Executive Summary

**Meeting Overview:**  
Data Center planning session with NT on November 17, 2025, to establish IT infrastructure and security strategy.

**Current Infrastructure:**
- 36 locations with 3,089 client machines, 102 physical servers, 233 VMs, and 119 systems
- Data accuracy issue identified: one site reported 2 machines but actually has 100+ machines

**Proposed Architecture:**
1. **Network Zones:** 5-zone design (Internet, Server, Campus, User, Management)
2. **Site Connectivity:** Hybrid SD-WAN and MPLS approach based on site size
3. **Security Layers:** Multi-layered defense from Spine to Server level
4. **Environment Separation:** VLAN-based separation for dev/UAT/production

**Key Recommendations from NT:**
- ✅ Prioritize Data Center implementation before branch expansion
- ✅ Use Centralized AD with API integration instead of direct AD integration on Day 1
- ❌ Do NOT implement ZTNA due to vendor dependency and staffing concerns
- ✅ Apply static port mapping only for critical systems
- ✅ Classify systems by priority: High/Medium/General zones

**Implementation Plan (Phase 0-4):**
- **Phase 0:** Preparation and PoC (2-3 months) - Select max 3 vendors
- **Phase 1:** Trial with ≤15% sample group (6-12 months)
- **Phase 2:** Adaptation and training (3-6 months)
- **Phase 3:** Expansion to 20-50% (6-9 months)
- **Phase 4:** Full coverage at 100% (6-12 months)

**Immediate Action Required:**
1. 🔴 **Critical:** Verify actual machine counts per site for accurate sizing
2. 🔴 **Critical:** Define Day 1 scope and prioritize procurement
3. 🟠 **Important:** Clarify ThaID authentication scope

**Risks:**
- Inaccurate data may impact budget and capacity planning
- Migration timeline concerns from stakeholder (คุณดอน) require clear roadmap
- Day 1 user resistance if changes are too disruptive

**Timeline Impact:**
- Total project duration: approximately 2-4 years for complete Phase 0-4
- Phase 0-1 (8-15 months) is the most critical for testing and refinement

**Key Takeaways:**
- Conservative, risk-mitigated approach using phased implementation
- Avoid premature adoption of complex technologies (e.g., ZTNA) without adequate team readiness
- Data Center foundation must be solid before branch expansion
- Data accuracy verification is paramount before project commencement

**Strategic Decisions:**
- **Technology Selection:** Proven, supportable technologies within team capabilities
- **Security Approach:** Layered defense with priority-based implementation
- **User Impact:** Minimize Day 1 disruption to ensure adoption
- **Vendor Strategy:** Avoid lock-in with ZTNA; maintain flexibility

**Success Factors:**
- Accurate infrastructure data collection
- Realistic timeline expectations
- Adequate training and staffing
- Phased rollout with continuous feedback
- Strong vendor partnership (NT) for architecture guidance

---

## 📎 Attachments / References

- **Architecture Diagram:** Pending from NT (พี่เกษม)
- **Site Survey Data:** 36 locations (requires verification)
- **System Inventory:** 119 systems (to be classified by priority)
- **TRD Security Guidelines:** VLAN separation for environments

---

## 🔄 Next Meeting

**Proposed Agenda:**
1. Review verified machine count data
2. Present finalized Data Center architecture diagram
3. Review Day 1 scope and procurement priorities
4. Discuss ThaID authentication integration
5. Review Phase 0 PoC vendor selection criteria
6. Address timeline concerns in detail

**Suggested Date:** Week of November 24, 2025

---

**Document Prepared By:** Meeting Notes Assistant  
**Document Version:** 1.0  
**Last Updated:** November 17, 2025  
**Status:** Draft - Pending Review
