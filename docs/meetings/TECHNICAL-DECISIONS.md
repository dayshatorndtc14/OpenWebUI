# Technical Decisions Log
# บันทึกการตัดสินใจทางเทคนิค

This document maintains a historical record of all technical and architectural decisions made across meetings.

---

## 2025-11-17: Data Center Infrastructure Planning with NT

**Meeting Reference:** [2025-11-17-datacenter-planning-nt.md](./2025-11/2025-11-17-datacenter-planning-nt.md)

### Architecture Decisions / การตัดสินใจด้านสถาปัตยกรรม

#### ADR-001: Network Zone Architecture (5-Zone Design)
- **Status:** ✅ Approved
- **Decision:** Implement 5-zone network architecture
- **Zones:** Internet, Server, Campus, User, Management
- **Rationale:** Proper network segmentation for security and management
- **Alternatives Considered:** 3-zone (rejected - insufficient segmentation), 7-zone (rejected - too complex)
- **Impact:** All network designs must follow this zoning model
- **Decided By:** พี่เกษม (NT), Technical Team
- **Date:** 2025-11-17

#### ADR-002: Data Center First Approach
- **Status:** ✅ Approved
- **Decision:** Prioritize Data Center implementation before branch expansion
- **Rationale:** Establish solid core foundation, reduce complexity, minimize risk
- **Alternatives Considered:** Parallel DC+Branch rollout (rejected - too risky)
- **Impact:** Branch implementation deferred until Data Center is stable
- **Decided By:** พี่เกษม (NT)
- **Date:** 2025-11-17

#### ADR-003: Hybrid SD-WAN and MPLS Connectivity
- **Status:** ✅ Approved
- **Decision:** Use hybrid SD-WAN and MPLS based on site size
  - Small (1-20 machines): 1 SD-WAN
  - Medium (20-90 machines): 1 SD-WAN + 1 MPLS
  - Campus (500+ machines): 2 SD-WAN + 2 MPLS
- **Rationale:** Balance cost, performance, reliability, and redundancy
- **Alternatives Considered:** MPLS-only (rejected - too expensive), SD-WAN-only (rejected - insufficient reliability)
- **Impact:** Procurement and site planning must follow this model
- **Decided By:** พี่เกษม (NT), Project Team
- **Date:** 2025-11-17

---

### Security Decisions / การตัดสินใจด้านความปลอดภัย

#### ADR-004: Layered Security Model (Spine to Server)
- **Status:** ✅ Approved
- **Decision:** Implement defense-in-depth security from Spine → Core FW → ToR FW → Server
- **Priority Order:** 
  1. Spine (highest priority)
  2. Core Firewall
  3. Top-of-Rack Firewall
  4. Server (lowest priority)
- **Rationale:** Multiple security layers provide comprehensive protection
- **Alternatives Considered:** Edge-only security (rejected - single point of failure)
- **Impact:** Budget must cover all security layers; implementation follows this sequence
- **Decided By:** พี่เกษม (NT)
- **Date:** 2025-11-17

#### ADR-005: Service Priority Zones
- **Status:** ✅ Approved
- **Decision:** Classify services into High/Medium/General priority zones
- **Rationale:** Focus security resources on most critical systems first
- **Alternatives Considered:** Uniform security for all (rejected - inefficient resource allocation)
- **Impact:** System classification required before security implementation
- **Action Required:** NT-2025-11-17-004 (classify systems)
- **Decided By:** พี่เกษม (NT)
- **Date:** 2025-11-17

#### ADR-006: Static Port Mapping for Critical Systems Only
- **Status:** ✅ Approved
- **Decision:** Use static port mapping only for critical systems; dynamic allocation for general services
- **Rationale:** Simplifies management while ensuring critical system availability
- **Alternatives Considered:** All static (rejected - management overhead), all dynamic (rejected - critical system risk)
- **Impact:** Critical systems must be identified and documented
- **Action Required:** NT-2025-11-17-008 (document port mapping requirements)
- **Decided By:** พี่เกษม (NT)
- **Date:** 2025-11-17

#### ADR-007: VLAN-Based Environment Separation (TRD Recommendation)
- **Status:** ✅ Approved
- **Decision:** Separate dev/UAT/production environments using VLANs
- **Rationale:** Security best practice; prevents cross-contamination
- **Recommended By:** TRD (Threat Research Division)
- **Alternatives Considered:** Physical separation (rejected - too expensive), no separation (rejected - security risk)
- **Impact:** All environments must be properly segregated
- **Decided By:** Security Team, พี่เกษม (NT)
- **Date:** 2025-11-17

---

### Authentication & Access Control Decisions / การตัดสินใจด้านการยืนยันตัวตนและการควบคุมการเข้าถึง

#### ADR-008: No AD Integration on Day 1
- **Status:** ✅ Approved
- **Decision:** Do NOT integrate Active Directory directly on Day 1
- **Rationale:** Minimize user resistance; reduce Day 1 complexity
- **Alternatives Considered:** Direct AD integration (rejected - high user resistance risk)
- **Impact:** Day 1 authentication must work without AD dependency
- **Decided By:** พี่เกษม (NT), Project Team
- **Date:** 2025-11-17

#### ADR-009: Centralized AD with API Integration
- **Status:** ✅ Approved
- **Decision:** Use centralized Active Directory for user management with API-based integration
- **Rationale:** Maintains central user control while avoiding direct Day 1 impact
- **Alternatives Considered:** Distributed AD (rejected - management complexity)
- **Impact:** API integration layer must be developed
- **Decided By:** พี่เกษม (NT)
- **Date:** 2025-11-17

#### ADR-010: No ZTNA Implementation
- **Status:** ✅ Approved (Do NOT Implement)
- **Decision:** Do NOT implement Zero Trust Network Access (ZTNA)
- **Rationale:** 
  - High vendor dependency and lock-in risk
  - Requires specialized staff capabilities not currently available
  - Agent vs agentless trade-offs create limitations
  - Organization not ready for ZTNA complexity
- **Alternatives Considered:** Agent-based ZTNA (rejected), Agentless ZTNA (rejected)
- **Impact:** Access control will use traditional methods with enhanced monitoring
- **Decided By:** พี่เกษม (NT), Management
- **Date:** 2025-11-17

#### ADR-011: ThaID Authentication Scope (Pending)
- **Status:** ⏳ Pending Clarification
- **Decision:** TBD - Awaiting clarification on ThaID usage scope
- **Action Required:** NT-2025-11-17-005 (clarify ThaID requirements)
- **Impact:** Authentication design cannot be finalized until this is clarified
- **Date:** 2025-11-17

---

### Implementation Strategy Decisions / การตัดสินใจด้านกลยุทธ์การดำเนินการ

#### ADR-012: Phase 0-4 POC Approach
- **Status:** ✅ Approved
- **Decision:** Follow structured Phase 0-4 implementation with POC validation
  - **Phase 0:** Preparation & PoC (2-3 months) - Max 3 vendors, select 1-2
  - **Phase 1:** Trial with ≤15% users (6-12 months)
  - **Phase 2:** Adaptation & training (3-6 months)
  - **Phase 3:** Expansion to 20-50% (6-9 months)
  - **Phase 4:** Full coverage 100% (6-12 months)
- **Total Duration:** 2-4 years
- **Rationale:** Risk mitigation through phased validation and learning
- **Alternatives Considered:** Big bang deployment (rejected - too risky), 2-phase only (rejected - insufficient validation)
- **Impact:** Timeline expectations, resource allocation, success metrics per phase
- **Decided By:** พี่เกษม (NT), Project Team
- **Date:** 2025-11-17

#### ADR-013: Maximum 3 Vendors for PoC
- **Status:** ✅ Approved
- **Decision:** Limit Phase 0 PoC to maximum 3 vendors, select 1-2 for trial
- **Rationale:** Balance thorough evaluation with manageable scope
- **Alternatives Considered:** Single vendor (rejected - no comparison), 5+ vendors (rejected - evaluation overhead)
- **Impact:** Vendor selection criteria must be defined upfront
- **Action Required:** NT-2025-11-17-007 (prepare PoC plan)
- **Decided By:** Project Team
- **Date:** 2025-11-17

#### ADR-014: Day 1 Scope Prioritization (Pending)
- **Status:** ⏳ Pending Definition
- **Decision:** TBD - Day 1 scope and purchase priorities need definition
- **Action Required:** NT-2025-11-17-002 (define Day 1 scope)
- **Impact:** Blocks procurement planning
- **Date:** 2025-11-17

---

## Decision Status Legend / คำอธิบายสถานะ

- ✅ **Approved** - อนุมัติแล้ว
- ⏳ **Pending** - รอการตัดสินใจ
- 🔄 **Under Review** - อยู่ระหว่างพิจารณา
- ❌ **Rejected** - ปฏิเสธ
- 🔄 **Superseded** - ถูกแทนที่ด้วยการตัดสินใจใหม่
- ⚠️ **Deprecated** - ไม่แนะนำให้ใช้แล้ว

---

## Decision Impact Matrix / เมทริกซ์ผลกระทบ

| ADR | Scope | Cost Impact | Timeline Impact | Risk Level |
|-----|-------|-------------|-----------------|------------|
| ADR-001 | Architecture | Medium | Low | Low |
| ADR-002 | Strategy | Low | High (extends branches) | Low |
| ADR-003 | Infrastructure | High | Medium | Medium |
| ADR-004 | Security | High | Medium | Medium |
| ADR-005 | Security | Low | Low | Low |
| ADR-006 | Operations | Low | Low | Low |
| ADR-007 | Security | Medium | Low | Low |
| ADR-008 | User Experience | Low | Low (reduces Day 1 risk) | Low |
| ADR-009 | Infrastructure | Medium | Medium | Medium |
| ADR-010 | Security | High (cost savings) | Low | Low (risk avoided) |
| ADR-011 | Authentication | TBD | TBD | Medium (pending) |
| ADR-012 | Strategy | High | Very High (2-4 years) | Low (phased reduces risk) |
| ADR-013 | Procurement | Low | Low | Low |
| ADR-014 | Procurement | TBD | High (blocks procurement) | High (pending) |

---

## Critical Dependencies / ความสัมพันธ์เชิงสาเหตุที่สำคัญ

```
ADR-002 (DC First) 
  └─> ADR-012 (Phase 0-4)
        └─> ADR-013 (3 Vendors)
              └─> ADR-014 (Day 1 Scope) [PENDING]

ADR-001 (5 Zones)
  ├─> ADR-004 (Layered Security)
  │     └─> ADR-005 (Priority Zones)
  │           └─> ADR-006 (Static Ports)
  └─> ADR-007 (VLAN Separation)

ADR-008 (No AD Day 1)
  └─> ADR-009 (Centralized AD/API)

ADR-010 (No ZTNA)
  └─> Traditional access control approach

ADR-011 (ThaID) [PENDING]
  └─> Authentication architecture [BLOCKED]
```

---

## Lessons Learned / บทเรียนที่ได้เรียนรู้

### From 2025-11-17 Meeting:

1. **Data Accuracy is Critical**
   - Lesson: Always verify infrastructure data before planning
   - Example: Site reported 2 machines, actually had 100+
   - Impact: Affects sizing, budget, procurement
   - Action: Mandatory data audit before any major planning (ADR-future)

2. **User Resistance Management**
   - Lesson: Day 1 changes should minimize user impact
   - Decision: ADR-008 (No AD on Day 1)
   - Approach: Backend improvements first, user-facing changes later

3. **Avoid Premature Technology Adoption**
   - Lesson: Don't implement technologies before organization is ready
   - Decision: ADR-010 (No ZTNA)
   - Rationale: Vendor lock-in, staff capability, complexity

4. **Phased Approach Reduces Risk**
   - Lesson: Big bang deployments are too risky for critical infrastructure
   - Decision: ADR-012 (Phase 0-4)
   - Benefit: Learning, adaptation, risk mitigation at each phase

---

## Change History / ประวัติการเปลี่ยนแปลง

| Date | ADR | Change Type | Description |
|------|-----|-------------|-------------|
| 2025-11-17 | ADR-001 to ADR-014 | Initial | First technical decisions from NT planning meeting |

---

**Document Owner:** Architecture Team  
**Last Updated:** November 17, 2025  
**Review Cycle:** After each major technical meeting  
**Next Review:** TBD after next planning session
