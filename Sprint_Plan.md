# The Living Existence Network (LEN)
## Sprint Plan v2.0 (Detailed Engineering Version)

---

# 1. Project Context

Project: The Living Existence Network (LEN)  
Methodology: Agile Scrum  
Sprint Duration: 2 Weeks (14 Days)  
Team Size: 5–7 Members  
Branching Strategy: GitFlow  
CI/CD: GitHub Actions  
Deployment: Docker + Kubernetes (Future Phase)

---

# 2. Team Structure & Roles

| Role | Responsibility |
|------|---------------|
| Product Owner | Define Vision, Approve Sprint Scope |
| Scrum Master | Facilitate Process, Remove Blockers |
| Backend Engineer | Core Modules Implementation |
| Network Engineer | Routing & Evolution Logic |
| Security Engineer | Encryption & Privacy |
| QA Engineer | Testing & Validation |
| DevOps | CI/CD & Deployment |

---

# 3. Sprint Cadence

Day 1: Sprint Planning  
Day 2–11: Development + Daily Standup  
Day 12–13: Testing + Integration  
Day 14: Sprint Review + Retrospective  

Daily Standup:
- What did you do yesterday?
- What will you do today?
- Any blockers?

---

# 4. Sprint Alpha – Core Infrastructure Foundation

## Objective

สร้างระบบ Node แบบกระจายศูนย์ที่รันได้จริง  
สามารถจำลองเครือข่ายพื้นฐานและส่งข้อความระหว่าง Node ได้

---

## 4.1 Sprint Goal

“LEN Node Runtime สามารถเชื่อมต่อกันและส่งข้อความผ่าน Basic Routing ได้”

---

## 4.2 Functional Scope

- Distributed Node Class
- Network State Model
- Basic Routing Logic
- Logging & Monitoring
- Unit Test Framework Setup

---

## 4.3 Architecture for Sprint Alpha

Modules:

- node/
- routing/
- network_state/
- logger/
- tests/

---

## 4.4 Detailed Task Breakdown

### SA-01: Node Runtime Skeleton

**Description:**  
สร้าง class Node สำหรับจำลอง distributed environment

```python
class Node:

    def __init__(self, node_id):
        self.node_id = node_id
        self.connected_nodes = []
        self.status = "ACTIVE"

    def connect(self, node):
        self.connected_nodes.append(node)

    def send(self, message):
        for node in self.connected_nodes:
            node.receive(message)

    def receive(self, message):
        print(f"[{self.node_id}] received:", message)
```
#### Acceptance Criteria:

- Node สามารถ connect ได้
- Node ส่งและรับข้อความได้
- ไม่มี runtime error

### SA-02: Network State Model

```python
class NetworkState:

    def __init__(self):
        self.nodes = {}
        self.routes = []

    def add_node(self, node):
        self.nodes[node.node_id] = node

    def add_route(self, source, target, latency):
        self.routes.append({
            "source": source,
            "target": target,
            "latency": latency
        })
```
