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
| Architect | นายธนดล ไชยศิลา | ออกแบบโครงสร้าง LEN ทั้งระบบ (Layer Architecture, Node Model, Protocol Design) | ตรวจสอบความสอดคล้องของเอกสาร | อนุมัติการเปลี่ยนแปลง Architecture |
| Specialist | นายกฤติธี ศรีใสย์ | พัฒนา Node Simulation, Routing Logic, ExistencePacket Handling | ปรับปรุง Performance | ตัดสินใจ Implementation ระดับ Code |
| Engineer | นางสาวปรายฝน ฮกเซ็ง | พัฒนา Evolution Engine, Awareness Logic | สร้าง Testing Scenario | อนุมัติ Logic Rule |
| DevOps | นายคมชาญ วรสาร | ตั้งค่า Repository, GitFlow, CI/CD | Simulation Environment Setup | ควบคุม Deployment Process |
| Tester/QA | นางสาวนันทพร ลุนทอง | จัดทำเอกสาร, Test Plan, Test Case | ตรวจสอบคุณภาพงาน Sprint | อนุมัติความครบถ้วนของเอกสาร |

---

# 3. การแบ่งความรับผิดชอบตาม Layer

## 3.1 Awareness Layer
**ผู้รับผิดชอบหลัก:** Engineer  
**ผู้สนับสนุน:** Specialist  

### หน้าที่
- พัฒนาโมเดล cognitive-state vector  
- คำนวณ intent_signature  
- จัดการค่า awareness threshold  

---

## 3.2 Existence Communication Layer
**ผู้รับผิดชอบหลัก:** Specialist  
**ผู้สนับสนุน:** Architect  

### หน้าที่
- พัฒนาระบบกระจายข้อความ (Message Broadcast)  
- พัฒนา Routing Protocol  
- ป้องกันการเกิด Infinite Loop  
- ปรับปรุงประสิทธิภาพด้าน Latency  

---

## 3.3 Evolution Engine
**ผู้รับผิดชอบหลัก:** Engineer  
**ผู้สนับสนุน:** Architect  

### หน้าที่
- กำหนดกฎการวิวัฒน์ (Evolution Rules)  
- พัฒนา Adaptive Routing Logic  
- จำลองพฤติกรรมของระบบ (System Behavior Simulation)  

---

## 3.4 Infrastructure Layer
**ผู้รับผิดชอบ:** DevOps  

### หน้าที่
- บริหารจัดการ GitHub Repository  
- วางกลยุทธ์การแตก Branch (GitFlow)  
- ตั้งค่า CI/CD (GitHub Actions)  
- ควบคุมสภาพแวดล้อมสำหรับ Simulation  

---

## 3.5 Documentation & Quality Control
**ผู้รับผิดชอบ:** Tester/QA  

### หน้าที่
- จัดทำ `Architecture_Spec.md`  
- จัดทำ `Implementation_Plan.md`  
- จัดทำ `Sprint_Plan.md`  
- ตรวจสอบรูปแบบ Markdown  
- ตรวจสอบและยืนยันผลการทดสอบ  

---

# 4. กระบวนการตัดสินใจและการยกระดับปัญหา (Decision & Escalation Flow)

1. ปัญหาด้านการพัฒนาเชิงเทคนิค → Specialist  
2. ปัญหาด้าน Logic/AI → Engineer  
3. ความขัดแย้งด้านสถาปัตยกรรม → Architect  
4. ปัญหาโครงสร้างพื้นฐาน → DevOps  
5. การอนุมัติเอกสาร → Tester/QA  

**ผู้มีอำนาจตัดสินใจขั้นสุดท้าย:** Architect  

---

# 5. กฎการทำงานร่วมกัน (Collaboration Rules)

- Daily Standup: 10–15 นาที  
- Sprint Review: สิ้นสุดรอบ 2 สัปดาห์  
- Pull Request Review: ต้องมีการอนุมัติอย่างน้อย 1 คน  
- การเปลี่ยนแปลงสถาปัตยกรรมหลัก: ต้องได้รับการอนุมัติจาก Architect  

---

# 6. ตารางความรับผิดชอบ (RACI Model)

| งาน | Architect | Specialist | Engineer | DevOps | Tester/QA |
|------|-----------|---------|------------|--------|----|
| ออกแบบสถาปัตยกรรม | R | C | C | C | I |
| พัฒนา Node | C | R | C | I | I |
| พัฒนา Evolution Logic | C | C | R | I | I |
| ตั้งค่า CI/CD | I | I | I | R | C |
| จัดทำเอกสาร | C | I | I | I | R |

**Legend:**  
- **R** = Responsible (ผู้รับผิดชอบหลักในการลงมือทำ)  
- **A** = Accountable (ผู้รับผิดชอบผลลัพธ์ขั้นสุดท้าย)  
- **C** = Consulted (ผู้ให้คำปรึกษา)  
- **I** = Informed (ผู้รับทราบข้อมูล)  
