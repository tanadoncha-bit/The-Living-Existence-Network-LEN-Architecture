# The Living Existence Network (LEN)
## Role and Responsibility Document
Version: 1.0  
Sprint: Alpha  
Methodology: Agile Scrum  

---

# 1. Team Members

| No. | Name | Student ID |
|----|------|------------|
| 1 | นายธนดล ไชยศิลา | 673380585-0 |
| 2 | นางสาวนันทพร ลุนทอง | 673380409-0 |
| 3 | นางสาวปรายฝน ฮกเซ็ง | 673380591-5 |
| 4 | นายกฤติธี ศรีใสย์ | 673380572-9 |
| 5 | นายคมชาญ วรสาร | 673380396-3 |

---

# 2. Role Assignment Matrix

| Role | Assigned To | Primary Responsibilities | Secondary Responsibilities | Decision Authority |
|------|------------|--------------------------|----------------------------|-------------------|
| System Architect | นายธนดล ไชยศิลา | ออกแบบโครงสร้าง LEN ทั้งระบบ (Layer Architecture, Node Model, Protocol Design) | ตรวจสอบความสอดคล้องของเอกสาร | อนุมัติการเปลี่ยนแปลง Architecture |
| Backend Engineer | นายกฤติธี ศรีใสย์ | พัฒนา Node Simulation, Routing Logic, ExistencePacket Handling | ปรับปรุง Performance | ตัดสินใจ Implementation ระดับ Code |
| AI / Logic Engineer | นางสาวปรายฝน ฮกเซ็ง | พัฒนา Evolution Engine, Awareness Logic | สร้าง Testing Scenario | อนุมัติ Logic Rule |
| Infrastructure & DevOps | นายคมชาญ วรสาร | ตั้งค่า Repository, GitFlow, CI/CD | Simulation Environment Setup | ควบคุม Deployment Process |
| QA & Documentation Lead | นางสาวนันทพร ลุนทอง | จัดทำเอกสาร, Test Plan, Test Case | ตรวจสอบคุณภาพงาน Sprint | อนุมัติความครบถ้วนของเอกสาร |

---

# 3. Responsibility Breakdown by Layer

## 3.1 Awareness Layer
Responsible: AI / Logic Engineer  
Support: Backend Engineer  

หน้าที่:
- พัฒนา cognitive-state vector model
- คำนวณ intent_signature
- จัดการ awareness threshold

---

## 3.2 Existence Communication Layer
Responsible: Backend Engineer  
Support: System Architect  

หน้าที่:
- พัฒนา Message broadcast
- Routing protocol
- Prevent infinite loop
- Latency optimization

---

## 3.3 Evolution Engine
Responsible: AI / Logic Engineer  
Support: System Architect  

หน้าที่:
- Define evolution rules
- Adaptive routing logic
- System behavior simulation

---

## 3.4 Infrastructure Layer
Responsible: Infrastructure & DevOps  

หน้าที่:
- GitHub repository management
- Branching strategy (GitFlow)
- CI/CD setup (GitHub Actions)
- Simulation environment control

---

## 3.5 Documentation & Quality Control
Responsible: QA & Documentation Lead  

หน้าที่:
- จัดทำ Architecture_Spec.md
- Implementation_Plan.md
- Sprint_Plan.md
- ตรวจสอบ Markdown formatting
- Validate test results

---

# 4. Decision & Escalation Flow

1. Technical Implementation Issue → Backend Engineer
2. Logic/AI Issue → AI Engineer
3. Architecture Conflict → System Architect
4. Infrastructure Problem → DevOps
5. Documentation Approval → QA Lead

Final escalation authority: System Architect

---

# 5. Collaboration Rules

- Daily Standup: 10–15 minutes
- Sprint Review: End of 2-week cycle
- Pull Request Review: ต้องมีอย่างน้อย 1 approval
- Major Architecture Change: ต้องได้รับการอนุมัติจาก Architect

---

# 6. Accountability Matrix (RACI Model)

| Task | Architect | Backend | AI Engineer | DevOps | QA |
|------|-----------|---------|------------|--------|----|
| Architecture Design | R | C | C | C | I |
| Node Implementation | C | R | C | I | I |
| Evolution Logic | C | C | R | I | I |
| CI/CD Setup | I | I | I | R | C |
| Documentation | C | I | I | I | R |

Legend:  
R = Responsible  
A = Accountable  
C = Consulted  
I = Informed  

---

# 7. Commitment Statement

ทีมงาน The Living Existence Network (LEN) ยืนยันว่าจะปฏิบัติตามบทบาทหน้าที่ที่ได้รับมอบหมาย และร่วมมือกันพัฒนา Sprint Alpha ให้สำเร็จตามเป้าหมายที่กำหนด

---
