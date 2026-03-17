# The Living Existence Network (LEN)

> "โครงสร้างดิจิทัลที่มีชีวิต — ปรับตัว วิวัฒน์ และปกป้องการดำรงอยู่ของผู้ใช้โดยอัตโนมัติ"

---

## ภาพรวม

**The Living Existence Network (LEN)** คือสถาปัตยกรรมเครือข่ายแบบกระจายศูนย์รุ่นใหม่ที่ก้าวข้ามโมเดล TCP/IP แบบดั้งเดิม

แทนที่จะเป็นเพียงระบบส่ง packet LEN ถูกออกแบบให้เป็นโครงสร้างที่ **มีชีวิต** — สามารถปรับ topology ตัวเองได้อัตโนมัติ ฟื้นฟูเมื่อเกิดความผิดพลาด และปกป้องความเป็นส่วนตัวในระดับสถาปัตยกรรม ไม่ใช่แค่ระดับ application

---

## คุณสมบัติหลัก

**Self-Evolving Topology**  
Evolution Engine วิเคราะห์สถานะเครือข่ายและปรับ topology อัตโนมัติ — ตัดเส้นทางที่ช้า เพิ่มเส้นทางใหม่เมื่อ traffic หนาแน่น โดยไม่ต้องหยุดระบบ

**Zero-Downtime Self-Healing**  
เมื่อ Node ล้มเหลว ระบบตรวจจับและหาเส้นทางสำรองในระดับมิลลิวินาที Node สามารถฟื้นฟูตัวเองผ่าน lifecycle: ACTIVE → DEGRADED → FAILED → HEALING → ACTIVE

**Structural Privacy**  
ข้อมูลผูกกับตัวตนโดยโครงสร้าง (Identity-Bound Encryption) ไม่สามารถเข้าถึงได้หากไม่ใช่เจ้าของ ไม่ต้องพึ่ง policy ระดับ application

**Consent-Before-Transmit**  
ไม่มี ExistencePacket ใดถูกส่งได้โดยไม่ได้รับการยินยอมจากผู้รับก่อน บังคับใช้ผ่าน ConsentManager ในระดับ protocol

**Multi-Reality Integration**  
รองรับการเชื่อมต่อข้าม Physical, Virtual และ Simulation layers พร้อมกัน

---

## สถาปัตยกรรม 5 ชั้น

```
Reality Integration Layer      ← เชื่อมโลกจริง โลกเสมือน และระบบจำลอง
Human Awareness Layer          ← ป้องกัน Cognitive Overload, บังคับ Consent
Existence Communication Layer  ← ExistencePacket + ETP Protocol
Evolution Engine               ← ปรับ topology อัตโนมัติ
Infrastructure Layer           ← Node runtime, Routing, Self-Healing
```

---

## โครงสร้าง Repository

| ไฟล์ | คำอธิบาย |
|------|-----------|
| `README.md` | ภาพรวมโปรเจกต์ (ไฟล์นี้) |
| `Architecture_Spec.md` | สเปกสถาปัตยกรรมระบบทั้งหมด |
| `Implementation_Plan.md` | แผนพัฒนาแบ่งเป็น 6 Phase |
| `Coding.md` | Source code หลัก + Unit Tests (25 tests) |
| `Sprint_Plan.md` | Sprint Alpha, Sprint 3 & Sprint 4 |
| `Role_and_Responsibility.md` | บทบาทและความรับผิดชอบของทีม |
| `test_results.md` | ผลการรัน Unit Tests จริง |
| `demo.html` | Interactive demo — เปิดใน browser ได้เลย |

---

## การติดตั้งและรันโค้ด

**ข้อกำหนด:** Python 3.10 ขึ้นไป ไม่ต้อง install library เพิ่ม

```bash
# 1. Clone repository
git clone https://github.com/tanadoncha-bit/The-Living-Existence-Network-LEN-Architecture.git
cd The-Living-Existence-Network-LEN-Architecture

# 2. รัน Demo + Unit Tests
python len_core.py
```

**ผลที่คาดหวัง:**
```
====================================================
  LEN (Living Existence Network) — Core Demo
====================================================

[ROUTING]   Alpha → Delta
            Path    : Alpha → Gamma → Delta
            Latency : 10.0ms
...
Ran 25 tests in 0.002s
OK
```

---

## Interactive Demo

เปิดไฟล์ `demo.html` ในเบราว์เซอร์ได้เลย ไม่ต้อง install อะไรเพิ่ม

Demo แสดง 6 ขั้นตอนแบบ step-by-step:
1. การสร้างเครือข่าย
2. การค้นหาเส้นทาง (Dijkstra Algorithm)
3. การจำลอง Node ล้มเหลว
4. การฟื้นฟูตัวเอง (Self-Healing)
5. Evolution Engine ตัดเส้นทางช้า
6. Consent Manager

---

## ผลการทดสอบ

| Test Suite | จำนวน Tests | ผลลัพธ์ |
|------------|-------------|---------|
| TestNodeLifecycle | 5 | ผ่านทั้งหมด |
| TestNetworkState | 14 | ผ่านทั้งหมด |
| TestEvolutionEngine | 2 | ผ่านทั้งหมด |
| TestConsentManager | 4 | ผ่านทั้งหมด |
| **รวม** | **25** | **25/25** |

ดูผลการทดสอบแบบละเอียดได้ที่ [`test_results.md`](./test_results.md)

---

## Sprint Progress

| Sprint | เป้าหมาย | สถานะ |
|--------|---------|-------|
| Sprint Alpha | Node Runtime, Basic Routing, Unit Tests | เสร็จสิ้น |
| Sprint 3 | Bug Fixes, Domain Mapping, CI/CD Pipeline | เสร็จสิ้น |
| Sprint 4 | Maturity Metrics, Ethics/Governance, MVP | เสร็จสิ้น |

---

## ทีมพัฒนา

| ชื่อ | รหัสนักศึกษา | บทบาท |
|------|-------------|-------|
| นายธนดล ไชยศิลา | 673380585-0 | Architect |
| นางสาวนันทพร ลุนทอง | 673380409-0 | Tester/QA |
| นางสาวปรายฝน ฮกเซ็ง | 673380591-5 | Engineer |
| นายกฤติธี ศรีใสย์ | 673380572-9 | Specialist |
| นายคมชาญ วรสาร | 673380396-3 | DevOps |

---

## Project Artifacts

- **GitHub Repository** — โค้ดและเอกสารทั้งหมด
- **YouTube VDO** — วิดีโออธิบายภาพรวม LEN
- **Short Film** — นำเสนอวิสัยทัศน์ LEN ผ่านการเล่าเรื่อง
- **NotebookLM** — เอกสารทั้งหมด พร้อม Audio Overview และ Quiz

---

*The Living Existence Network — Digital Infrastructure สำหรับอารยธรรมยุคถัดไป*
