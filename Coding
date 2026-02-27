```python

import heapq
import unittest

# ==========================================
# 1. CORE SYSTEM (LEN Network Logic)
# ==========================================
class Node:
    def __init__(self, node_id):
        self.node_id = node_id
        self.status = "ACTIVE"

class NetworkState:
    def __init__(self):
        self.nodes = {}
        self.edges = {} # เก็บข้อมูลการเชื่อมต่อ {node_id: {neighbor_id: latency}}

    def add_node(self, node_id):
        self.nodes[node_id] = Node(node_id)
        self.edges[node_id] = {}

    def add_edge(self, node1, node2, latency):
        if node1 in self.nodes and node2 in self.nodes:
            self.edges[node1][node2] = latency
            self.edges[node2][node1] = latency # การเชื่อมต่อแบบ 2 ทาง (Undirected)

    def find_best_route(self, start, end):
        """หาเส้นทางที่ Latency ต่ำที่สุดด้วย Dijkstra Algorithm"""
        if start not in self.nodes or end not in self.nodes:
            raise ValueError("RouteNotFoundError: Source or Target node does not exist.")

        distances = {n: float('inf') for n in self.nodes}
        distances[start] = 0
        pq = [(0, start)]
        previous = {n: None for n in self.nodes}

        while pq:
            current_dist, current_node = heapq.heappop(pq)

            if current_node == end:
                path = []
                while current_node:
                    path.append(current_node)
                    current_node = previous[current_node]
                return path[::-1], current_dist # คืนค่า เส้นทาง และ Latency รวม

            if current_dist > distances[current_node]:
                continue

            for neighbor, weight in self.edges[current_node].items():
                distance = current_dist + weight
                if distance < distances[neighbor]:
                    distances[neighbor] = distance
                    previous[neighbor] = current_node
                    heapq.heappush(pq, (distance, neighbor))

        raise ValueError("RouteNotFoundError: No path exists between nodes.")

# ==========================================
# 2. UNIT TESTING (QA Validation)
# ==========================================
class TestLENNetwork(unittest.TestCase):
    
    def setUp(self):
        """รันก่อนเริ่มแต่ละ Test: สร้าง Network เปล่าๆ และเพิ่ม Node พื้นฐาน"""
        self.net = NetworkState()
        self.net.add_node("N1")
        self.net.add_node("N2")
        self.net.add_node("N3")

    def test_sa_01_add_node(self):
        """TC-SA-01: ตรวจสอบการสร้าง Node"""
        self.assertIn("N1", self.net.nodes)
        self.assertEqual(self.net.nodes["N1"].status, "ACTIVE")

    def test_sa_02_add_edge(self):
        """TC-SA-05: ตรวจสอบการบันทึก Edge และ Latency"""
        self.net.add_edge("N1", "N2", 15)
        self.assertEqual(self.net.edges["N1"]["N2"], 15)
        self.assertEqual(self.net.edges["N2"]["N1"], 15)

    def test_sa_03_routing_best_path(self):
        """TC-SA-03: ตรวจสอบ Routing (ต้องเลือกทางอ้อมที่เร็วกว่าทางตรง)"""
        # N1 ไป N3 ทางตรงใช้เวลา 50ms
        self.net.add_edge("N1", "N3", 50)
        # N1 ไป N2 (10ms) และ N2 ไป N3 (10ms) รวม 20ms
        self.net.add_edge("N1", "N2", 10)
        self.net.add_edge("N2", "N3", 10)
        
        path, total_latency = self.net.find_best_route("N1", "N3")
        
        self.assertEqual(path, ["N1", "N2", "N3"]) # คาดหวังว่าต้องวิ่งผ่าน N2
        self.assertEqual(total_latency, 20)

    def test_sa_04_routing_unreachable(self):
        """TC-SA-04: ตรวจสอบการจัดการ Error เมื่อไม่มีเส้นทาง (Edge Case)"""
        self.net.add_node("N4") # N4 ลอยๆ ไม่ได้เชื่อมกับใคร
        
        with self.assertRaises(ValueError) as context:
            self.net.find_best_route("N1", "N4")
        self.assertTrue("RouteNotFoundError" in str(context.exception))

# ==========================================
# 3. RUN SIMULATION & TESTS
# ==========================================
if __name__ == '__main__':
    print("==================================================")
    print(" LEN (Living Existence Network) - Core Demo")
    print("==================================================")
    
    # 1. รันการจำลองให้ดูแบบ Text (Demo)
    net = NetworkState()
    for n in ["A", "B", "C", "D"]: net.add_node(n)
    net.add_edge("A", "B", 10)
    net.add_edge("B", "D", 20)
    net.add_edge("A", "C", 5)
    net.add_edge("C", "D", 5) # เส้น A->C->D เร็วกว่า (10ms)

    path, cost = net.find_best_route("A", "D")
    print(f"[DEMO] Sending packet A -> D")
    print(f"[DEMO] Selected Path: {' -> '.join(path)} (Total Latency: {cost}ms)")
    
    print("\n==================================================")
    print(" RUNNING UNIT TESTS (QA Validation)")
    print("==================================================\n")
    
    # 2. รัน Unit Test ทั้งหมด
    unittest.main(argv=['first-arg-is-ignored'], exit=False)
