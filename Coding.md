```python
import heapq
import logging
import unittest

logging.basicConfig(level=logging.INFO, format="[%(levelname)s] %(message)s")
logger = logging.getLogger("LEN")


# ==========================================
# 1. CORE MODELS
# ==========================================

class NodeStatus:
    ACTIVE   = "ACTIVE"
    DEGRADED = "DEGRADED"
    FAILED   = "FAILED"
    HEALING  = "HEALING"


class Node:
    """
    Represents a single node in the LEN distributed network.
    Each node tracks its own status and can be updated
    as the network evolves.
    """

    def __init__(self, node_id: str):
        self.node_id = node_id
        self.status  = NodeStatus.ACTIVE

    def update_status(self, new_status: str):
        logger.info(f"Node {self.node_id}: {self.status} → {new_status}")
        self.status = new_status

    def is_available(self) -> bool:
        return self.status in (NodeStatus.ACTIVE, NodeStatus.DEGRADED)

    def __repr__(self):
        return f"Node({self.node_id}, {self.status})"


class NetworkState:
    """
    Maintains the full topology of the LEN network:
    nodes and weighted bidirectional edges (latency in ms).

    Routing uses Dijkstra's algorithm with visited-node tracking
    to guarantee termination on any graph, including cycles.
    """

    def __init__(self):
        self.nodes: dict = {}
        self.edges: dict = {}

    # ------ topology management ------

    def add_node(self, node_id: str) -> Node:
        node = Node(node_id)
        self.nodes[node_id] = node
        self.edges[node_id] = {}
        return node

    def remove_node(self, node_id: str):
        if node_id not in self.nodes:
            raise KeyError(f"Node '{node_id}' does not exist.")
        del self.nodes[node_id]
        del self.edges[node_id]
        for neighbors in self.edges.values():
            neighbors.pop(node_id, None)

    def add_edge(self, node1: str, node2: str, latency: float):
        if node1 not in self.nodes or node2 not in self.nodes:
            raise KeyError("Both nodes must exist before adding an edge.")
        if latency < 0:
            raise ValueError("Latency cannot be negative.")
        self.edges[node1][node2] = latency
        self.edges[node2][node1] = latency

    def remove_edge(self, node1: str, node2: str):
        self.edges[node1].pop(node2, None)
        self.edges[node2].pop(node1, None)

    # ------ routing ------

    def find_best_route(self, start: str, end: str):
        """
        Returns (path, total_latency) for the lowest-latency route
        between start and end using Dijkstra's algorithm.

        Only traverses ACTIVE or DEGRADED nodes.
        Raises ValueError if no path exists.
        """
        if start not in self.nodes:
            raise ValueError(f"RouteNotFoundError: Source node '{start}' does not exist.")
        if end not in self.nodes:
            raise ValueError(f"RouteNotFoundError: Target node '{end}' does not exist.")

        dist        = {n: float("inf") for n in self.nodes}
        dist[start] = 0
        prev        = {n: None for n in self.nodes}
        visited     = set()
        pq          = [(0.0, start)]

        while pq:
            current_dist, current = heapq.heappop(pq)

            if current in visited:
                continue
            visited.add(current)

            if current == end:
                return self._reconstruct_path(prev, end), current_dist

            if not self.nodes[current].is_available():
                continue

            for neighbor, weight in self.edges[current].items():
                if neighbor in visited:
                    continue
                if not self.nodes[neighbor].is_available():
                    continue
                new_dist = current_dist + weight
                if new_dist < dist[neighbor]:
                    dist[neighbor] = new_dist
                    prev[neighbor] = current
                    heapq.heappush(pq, (new_dist, neighbor))

        raise ValueError(
            f"RouteNotFoundError: No path exists between '{start}' and '{end}'."
        )

    def _reconstruct_path(self, prev: dict, end: str) -> list:
        path, node = [], end
        while node is not None:
            path.append(node)
            node = prev[node]
        return path[::-1]

    # ------ self-healing ------

    def heal_node(self, node_id: str):
        """Transition a FAILED node back to ACTIVE via HEALING state."""
        node = self.nodes.get(node_id)
        if node is None:
            raise KeyError(f"Node '{node_id}' does not exist.")
        node.update_status(NodeStatus.HEALING)
        node.update_status(NodeStatus.ACTIVE)


# ==========================================
# 2. EVOLUTION ENGINE
# ==========================================

EFFICIENCY_THRESHOLD  = 100.0   # ms — routes above this are pruned
TRAFFIC_DENSITY_LIMIT = 5       # edges — spawn new path above this

class EvolutionEngine:
    """
    Monitors network state and autonomously adapts topology:
    - Prunes inefficient routes
    - Signals when traffic density requires a new path
    """

    def evolve(self, network: NetworkState):
        self._prune_slow_routes(network)
        self._handle_high_density(network)

    def _prune_slow_routes(self, network: NetworkState):
        to_remove = []
        for node_id, neighbors in network.edges.items():
            for neighbor, latency in neighbors.items():
                if latency > EFFICIENCY_THRESHOLD:
                    to_remove.append((node_id, neighbor))
        for n1, n2 in to_remove:
            if n2 in network.edges.get(n1, {}):
                network.remove_edge(n1, n2)
                logger.info(f"Evolution: pruned slow edge {n1}↔{n2}")

    def _handle_high_density(self, network: NetworkState):
        total_edges = sum(len(v) for v in network.edges.values()) // 2
        if total_edges > TRAFFIC_DENSITY_LIMIT:
            logger.info(
                f"Evolution: high density ({total_edges} edges) — "
                "new path should be spawned."
            )


# ==========================================
# 3. CONSENT MANAGER
# ==========================================

class ConsentManager:
    """
    Enforces the LEN Ethics Framework:
    no ExistencePacket may be transmitted without explicit consent.
    """

    def __init__(self):
        self._registry: dict = {}

    def grant(self, receiver_id: str, sender_id: str):
        """Receiver grants permission to accept packets from sender."""
        self._registry.setdefault(receiver_id, {})[sender_id] = True

    def revoke(self, receiver_id: str, sender_id: str):
        """Receiver withdraws permission."""
        self._registry.setdefault(receiver_id, {})[sender_id] = False

    def verify(self, sender_id: str, receiver_id: str) -> bool:
        """ETP calls this before every transmission."""
        return self._registry.get(receiver_id, {}).get(sender_id, False)


# ==========================================
# 4. UNIT TESTS
# ==========================================

class TestNodeLifecycle(unittest.TestCase):

    def test_default_status_is_active(self):
        """TC-NL-01: New node starts as ACTIVE"""
        node = Node("N1")
        self.assertEqual(node.status, NodeStatus.ACTIVE)

    def test_update_status(self):
        """TC-NL-02: Status transitions correctly"""
        node = Node("N1")
        node.update_status(NodeStatus.DEGRADED)
        self.assertEqual(node.status, NodeStatus.DEGRADED)

    def test_is_available_when_active(self):
        """TC-NL-03: ACTIVE node is available for routing"""
        self.assertTrue(Node("N1").is_available())

    def test_is_available_when_degraded(self):
        """TC-NL-04: DEGRADED node is still available (degraded but reachable)"""
        node = Node("N1")
        node.update_status(NodeStatus.DEGRADED)
        self.assertTrue(node.is_available())

    def test_not_available_when_failed(self):
        """TC-NL-05: FAILED node must be excluded from routing"""
        node = Node("N1")
        node.update_status(NodeStatus.FAILED)
        self.assertFalse(node.is_available())


class TestNetworkState(unittest.TestCase):

    def setUp(self):
        self.net = NetworkState()
        for n in ["N1", "N2", "N3", "N4"]:
            self.net.add_node(n)

    # --- topology ---

    def test_add_node(self):
        """TC-NS-01: Node is stored and starts ACTIVE"""
        self.assertIn("N1", self.net.nodes)
        self.assertEqual(self.net.nodes["N1"].status, NodeStatus.ACTIVE)

    def test_add_edge_bidirectional(self):
        """TC-NS-02: Edge is stored in both directions"""
        self.net.add_edge("N1", "N2", 15)
        self.assertEqual(self.net.edges["N1"]["N2"], 15)
        self.assertEqual(self.net.edges["N2"]["N1"], 15)

    def test_remove_edge(self):
        """TC-NS-03: Edge removal clears both directions"""
        self.net.add_edge("N1", "N2", 10)
        self.net.remove_edge("N1", "N2")
        self.assertNotIn("N2", self.net.edges["N1"])
        self.assertNotIn("N1", self.net.edges["N2"])

    def test_remove_node_cleans_edges(self):
        """TC-NS-04: Removing a node also removes all its edges"""
        self.net.add_edge("N1", "N2", 10)
        self.net.remove_node("N2")
        self.assertNotIn("N2", self.net.nodes)
        self.assertNotIn("N2", self.net.edges["N1"])

    def test_add_edge_unknown_node_raises(self):
        """TC-NS-05: Edge to non-existent node raises KeyError"""
        with self.assertRaises(KeyError):
            self.net.add_edge("N1", "GHOST", 10)

    def test_negative_latency_raises(self):
        """TC-NS-06: Negative latency is rejected"""
        with self.assertRaises(ValueError):
            self.net.add_edge("N1", "N2", -5)

    # --- routing ---

    def test_direct_route(self):
        """TC-NS-07: Single-hop route returns correct path and cost"""
        self.net.add_edge("N1", "N2", 10)
        path, cost = self.net.find_best_route("N1", "N2")
        self.assertEqual(path, ["N1", "N2"])
        self.assertEqual(cost, 10)

    def test_prefers_indirect_low_latency_route(self):
        """TC-NS-08: Dijkstra selects indirect path when it is faster"""
        self.net.add_edge("N1", "N3", 50)   # direct but slow
        self.net.add_edge("N1", "N2", 10)
        self.net.add_edge("N2", "N3", 10)   # indirect but fast (20ms total)
        path, cost = self.net.find_best_route("N1", "N3")
        self.assertEqual(path, ["N1", "N2", "N3"])
        self.assertEqual(cost, 20)

    def test_no_path_raises(self):
        """TC-NS-09: Isolated node raises RouteNotFoundError"""
        with self.assertRaises(ValueError) as ctx:
            self.net.find_best_route("N1", "N4")
        self.assertIn("RouteNotFoundError", str(ctx.exception))

    def test_unknown_source_raises(self):
        """TC-NS-10: Non-existent source node raises ValueError"""
        with self.assertRaises(ValueError):
            self.net.find_best_route("GHOST", "N1")

    def test_cyclic_graph_no_infinite_loop(self):
        """TC-NS-11: Cyclic topology terminates correctly (no infinite loop)"""
        self.net.add_edge("N1", "N2", 5)
        self.net.add_edge("N2", "N3", 5)
        self.net.add_edge("N3", "N1", 5)
        path, cost = self.net.find_best_route("N1", "N3")
        self.assertEqual(cost, 5)

    def test_skips_failed_node(self):
        """TC-NS-12: FAILED node is bypassed, forcing detour"""
        self.net.add_edge("N1", "N2", 5)
        self.net.add_edge("N2", "N3", 5)
        self.net.add_edge("N1", "N3", 100)
        self.net.nodes["N2"].update_status(NodeStatus.FAILED)
        path, cost = self.net.find_best_route("N1", "N3")
        self.assertEqual(path, ["N1", "N3"])
        self.assertEqual(cost, 100)

    # --- self-healing ---

    def test_heal_node(self):
        """TC-NS-13: Healed node returns to ACTIVE status"""
        self.net.nodes["N1"].update_status(NodeStatus.FAILED)
        self.net.heal_node("N1")
        self.assertEqual(self.net.nodes["N1"].status, NodeStatus.ACTIVE)

    def test_heal_unknown_node_raises(self):
        """TC-NS-14: Healing non-existent node raises KeyError"""
        with self.assertRaises(KeyError):
            self.net.heal_node("GHOST")


class TestEvolutionEngine(unittest.TestCase):

    def setUp(self):
        self.net = NetworkState()
        for n in ["A", "B", "C"]:
            self.net.add_node(n)
        self.engine = EvolutionEngine()

    def test_prunes_slow_edges(self):
        """TC-EE-01: Edges above EFFICIENCY_THRESHOLD are removed"""
        self.net.add_edge("A", "B", 200)
        self.engine.evolve(self.net)
        self.assertNotIn("B", self.net.edges["A"])

    def test_keeps_fast_edges(self):
        """TC-EE-02: Edges below threshold are preserved"""
        self.net.add_edge("A", "B", 50)
        self.engine.evolve(self.net)
        self.assertIn("B", self.net.edges["A"])


class TestConsentManager(unittest.TestCase):

    def setUp(self):
        self.cm = ConsentManager()

    def test_default_no_consent(self):
        """TC-CM-01: No consent exists by default"""
        self.assertFalse(self.cm.verify("Alice", "Bob"))

    def test_grant_consent(self):
        """TC-CM-02: Granting consent allows transmission"""
        self.cm.grant("Bob", "Alice")
        self.assertTrue(self.cm.verify("Alice", "Bob"))

    def test_revoke_consent(self):
        """TC-CM-03: Revoking consent blocks transmission"""
        self.cm.grant("Bob", "Alice")
        self.cm.revoke("Bob", "Alice")
        self.assertFalse(self.cm.verify("Alice", "Bob"))

    def test_consent_is_directional(self):
        """TC-CM-04: Consent is one-directional (A→B does not imply B→A)"""
        self.cm.grant("Bob", "Alice")
        self.assertFalse(self.cm.verify("Bob", "Alice"))


# ==========================================
# 5. DEMO SIMULATION
# ==========================================

def run_demo():
    print("=" * 52)
    print("  LEN (Living Existence Network) — Core Demo")
    print("=" * 52)

    net = NetworkState()
    for n in ["Alpha", "Beta", "Gamma", "Delta"]:
        net.add_node(n)

    net.add_edge("Alpha", "Beta",  10)
    net.add_edge("Beta",  "Delta", 20)
    net.add_edge("Alpha", "Gamma",  5)
    net.add_edge("Gamma", "Delta",  5)   # Alpha→Gamma→Delta = 10ms (fastest)

    path, cost = net.find_best_route("Alpha", "Delta")
    print(f"\n[ROUTING]   Alpha → Delta")
    print(f"            Path    : {' → '.join(path)}")
    print(f"            Latency : {cost}ms")

    print("\n[FAILURE]   Simulating Gamma node failure...")
    net.nodes["Gamma"].update_status(NodeStatus.FAILED)
    path2, cost2 = net.find_best_route("Alpha", "Delta")
    print(f"[REROUTE]   New path: {' → '.join(path2)} ({cost2}ms)")

    print("\n[HEALING]   Recovering Gamma...")
    net.heal_node("Gamma")
    print(f"            Gamma status: {net.nodes['Gamma'].status}")

    print("\n[EVOLUTION] Running Evolution Engine...")
    net.add_edge("Alpha", "Delta", 999)
    EvolutionEngine().evolve(net)
    pruned = "Delta" not in net.edges["Alpha"]
    print(f"            Slow edge Alpha↔Delta pruned: {pruned}")

    print("\n[CONSENT]   Testing Consent Manager...")
    cm = ConsentManager()
    cm.grant("Delta", "Alpha")
    print(f"            Alpha → Delta allowed: {cm.verify('Alpha', 'Delta')}")
    cm.revoke("Delta", "Alpha")
    print(f"            After revoke          : {cm.verify('Alpha', 'Delta')}")
    print()


if __name__ == "__main__":
    run_demo()

    print("=" * 52)
    print("  RUNNING UNIT TESTS")
    print("=" * 52 + "\n")

    loader = unittest.TestLoader()
    suite  = unittest.TestSuite()
    for cls in [
        TestNodeLifecycle,
        TestNetworkState,
        TestEvolutionEngine,
        TestConsentManager,
    ]:
        suite.addTests(loader.loadTestsFromTestCase(cls))

    runner = unittest.TextTestRunner(verbosity=2)
    runner.run(suite)
```
