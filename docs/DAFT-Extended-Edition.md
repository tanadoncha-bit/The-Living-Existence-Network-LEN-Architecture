# DAFT: แกนกลางทางคณิตศาสตร์ของ The Living Existence Network
## Dyadic Attention Field Theory — ฉบับขยายสำหรับ LEN

---

### 1. ภาพรวม

The Living Existence Network ใช้ **Dyadic Attention Field Theory (DAFT)** เป็น mathematical backbone หลักในการควบคุมพฤติกรรมของเครือข่ายทั้งหมด ตั้งแต่การ classify ความสัมพันธ์ระหว่าง Node ไปจนถึงการตัดสินใจของ Evolution Engine

DAFT ถูกกำหนดด้วยพารามิเตอร์เพียง 3 ตัว:

```
α  (alpha)   — coupling strength: ความแข็งแกร่งของการเชื่อมต่อระหว่าง entity
λ  (lambda)  — resolution depth: ระดับความละเอียดในการสังเกต
d  (d)       — dimensionality: จำนวนมิติของ field space

Canonical values: α = 1, λ = 3, d = 1
ħ_DAFT = α²/λ = 1/3
```

---

### 2. สถานะการดำรงอยู่ 4 รูปแบบ

DAFT จำแนกความสัมพันธ์ระหว่าง Node คู่ใดๆ ด้วย operator **O₄ = |xᵢ| − |xⱼ|**:

| สถานะ | เงื่อนไข | ความหมายใน LEN |
|--------|----------|----------------|
| **PURE** | O₄ = 0 | Node คู่นั้น balanced — การเชื่อมต่อเหมาะสมที่สุด |
| **CONSTRUCTIVE** | \|xᵢ\| > \|xⱼ\| | Node i dominant — asymmetric แต่ยังทำงานได้ |
| **DESTRUCTIVE** | \|xᵢ\| < \|xⱼ\| | Node j dominant — อาจเป็น bottleneck |
| **BOUNDARY** | xᵢ · xⱼ = 0 | Node ใดตัวหนึ่ง = 0 — FAILED หรือ isolated |

**PURE state คือ global attractor** — Evolution Engine ออกแบบมาเพื่อผลักเครือข่ายเข้าสู่ PURE state อยู่เสมอ

---

### 3. กฎพลวัต 2 ข้อ

**Law 1 — Asymmetry Decay:**
```
O₄(t) = O₄(0) · e^(−t/τ)
```
ความไม่สมดุลระหว่าง Node คู่ใดๆ สลายตัวแบบ exponential เสมอ
Evolution Engine ใช้กฎนี้ทำนายว่า route จะ converge กลับสู่ PURE เมื่อไหร่ก่อนตัดสินใจ prune

**Law 2 — Resolution Growth:**
```
λ(t) = √(λ₀² + O₆ · t)
```
ความละเอียดของเครือข่ายเติบโตแบบ √t — cognitive state vector ของ Human Awareness Layer ขยาย dimension ตามกฎนี้

---

### 4. DAFT Uncertainty Principle

```
[Ô₄, Ô₆] = i · α²/λ = i · ħ_DAFT

ħ_DAFT = 1/3  (at canonical α=1, λ=3)
```

ความหมายใน LEN: ไม่สามารถวัด asymmetry (O₄) และ separation (O₆) ของ Node คู่ใดๆ ได้พร้อมกันอย่างแม่นยำ ยิ่ง λ สูง uncertainty ยิ่งน้อย

---

### 5. การนำ DAFT ไปใช้ใน LEN Components

| LEN Component | DAFT Operator | การนำไปใช้ |
|---------------|--------------|------------|
| Infrastructure Layer | O₆ = latency metric | วัดระยะห่างระหว่าง Node |
| Evolution Engine | O₄ decay law | ทำนายเวลาที่ควร prune edge |
| Human Awareness Layer | C_PURE = √(2/3) ≈ 0.817 | threshold ความพร้อมรับข้อมูล |
| Existence Communication Layer | O₅ = self-reference | entityId ของแต่ละ Entity |

---

### 6. Cognitive Readiness Thresholds

Human Awareness Layer ใช้ค่า threshold จาก ħ_DAFT โดยตรง:

```
C_PURE     = √(1 − ħ_DAFT) = √(2/3) ≈ 0.817  →  ALLOW transmission
C_BOUNDARY = √(ħ_DAFT)     = √(1/3) ≈ 0.577  →  minimum floor

coherence ≥ C_PURE     → ALLOW   (PURE state)
coherence ≥ C_BOUNDARY → THROTTLE (CONSTRUCTIVE state)
coherence < C_BOUNDARY → BLOCK   (BOUNDARY state)
```

รายละเอียดสูตรคณิตศาสตร์ครบถ้วนอยู่ใน [`Formalization.md`](./docs/Formalization.md)
