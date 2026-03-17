# The Living Existence Network (LEN)
## คู่มือเอกสาร — Document Guide

เอกสารในโปรเจกต์นี้ถูกออกแบบสำหรับผู้อ่านต่างกลุ่ม อ่านตามลำดับที่เหมาะกับคุณ:

---

## สำหรับทุกคน — เริ่มที่นี่

| เอกสาร | เหมาะสำหรับ | เนื้อหา |
|--------|------------|---------|
| [`README.md`](../README.md) | ทุกคน | ภาพรวมโปรเจกต์ วิธีรันโค้ด ผลทดสอบ |
| [`special-edition.md`](./special-edition.md) | ผู้บริหาร / นักลงทุน / ผู้สนใจทั่วไป | ปรัชญาและวิสัยทัศน์ของ LEN ไม่มีสมการ |
| [`demo.html`](../demo.html) | ทุกคน | Interactive demo เปิดใน browser ได้เลย |

---

## สำหรับวิศวกรและนักพัฒนา

| เอกสาร | เนื้อหา |
|--------|---------|
| [`Architecture_Spec.md`](./Architecture_Spec.md) | สเปกสถาปัตยกรรมทั้งระบบ 6 layers, protocol, data structures |
| [`Implementation_Plan.md`](./Implementation_Plan.md) | แผนพัฒนา 6 Phase พร้อม deliverables และ timeline |
| [`Coding.md`](../src/Coding.md) | Source code หลัก + Unit Tests 34 tests |
| [`test_results.md`](../src/test_results.md) | ผลรัน Unit Tests จริง 34/34 passed |

---

## สำหรับนักวิจัยและนักทฤษฎี

| เอกสาร | เนื้อหา |
|--------|---------|
| [`Formalization.md`](./Formalization.md) | Mathematical formalization ครบถ้วน — DAFT operators, สูตร, domain mapping |
| [`DAFT-Extended-Edition.md`](./DAFT-Extended-Edition.md) | อธิบาย DAFT theory ที่เป็น backbone ของ LEN แบบอ่านง่าย |
| [`Book_Theoretical.md`](./Book_Theoretical.md) | หนังสือทฤษฎีฉบับเต็ม ครอบคลุมทั้ง 10 บท |

---

## สำหรับทีมพัฒนา

| เอกสาร | เนื้อหา |
|--------|---------|
| [`Sprint_Plan.md`](../planning/Sprint_Plan.md) | Sprint Alpha, Sprint 3, Sprint 4 — task breakdown ละเอียด |
| [`Role_and_Responsibility.md`](../planning/Role_and_Responsibility.md) | RACI matrix และ decision authority |

---

## แผนผังความสัมพันธ์ของเอกสาร

```
README.md  ←  จุดเริ่มต้น
    │
    ├── อยากเข้าใจ "ทำไม"  →  special-edition.md
    │
    ├── อยากเข้าใจ "อะไร"  →  Architecture_Spec.md
    │                              └── ต้องการ math →  Formalization.md
    │                                                        └── ต้องการ theory →  Book_Theoretical.md
    │
    ├── อยากพัฒนา "ยังไง"  →  Implementation_Plan.md
    │                              └── ดูโค้ดจริง  →  Coding.md
    │
    └── อยากเห็น demo      →  demo.html
```
