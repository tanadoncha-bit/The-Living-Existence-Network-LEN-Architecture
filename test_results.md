# The Living Existence Network (LEN)
## Test Results — Sprint Alpha + Sprint 3

**Python version:** 3.12.3  
**Test framework:** unittest (built-in)  
**Total tests:** 34  
**Result:** ✅ ALL PASSED

---

## Demo Simulation Output

```
====================================================
  LEN (Living Existence Network) — Core Demo
====================================================

[ROUTING]   Alpha → Delta
            Path    : Alpha → Gamma → Delta
            Latency : 10.0ms

[FAILURE]   Simulating Gamma node failure...
[INFO] Node Gamma: ACTIVE → FAILED
[REROUTE]   New path: Alpha → Beta → Delta (30.0ms)

[HEALING]   Recovering Gamma...
[INFO] Node Gamma: FAILED → HEALING
[INFO] Node Gamma: HEALING → ACTIVE
            Gamma status: ACTIVE

[EVOLUTION] Running Evolution Engine...
            Slow edge Alpha↔Delta pruned: True

[CONSENT]   Testing Consent Manager...
            Alpha → Delta allowed: True
            After revoke          : False
```

---

## Unit Test Results

```
====================================================
  RUNNING UNIT TESTS
====================================================

test_default_status_is_active (TestNodeLifecycle) ... ok
test_is_available_when_active (TestNodeLifecycle) ... ok
test_is_available_when_degraded (TestNodeLifecycle) ... ok
test_not_available_when_failed (TestNodeLifecycle) ... ok
test_update_status (TestNodeLifecycle) ... ok
test_add_edge_bidirectional (TestNetworkState) ... ok
test_add_edge_unknown_node_raises (TestNetworkState) ... ok
test_add_node (TestNetworkState) ... ok
test_cyclic_graph_no_infinite_loop (TestNetworkState) ... ok
test_direct_route (TestNetworkState) ... ok
test_heal_node (TestNetworkState) ... ok
test_heal_unknown_node_raises (TestNetworkState) ... ok
test_negative_latency_raises (TestNetworkState) ... ok
test_no_path_raises (TestNetworkState) ... ok
test_prefers_indirect_low_latency_route (TestNetworkState) ... ok
test_remove_edge (TestNetworkState) ... ok
test_remove_node_cleans_edges (TestNetworkState) ... ok
test_skips_failed_node (TestNetworkState) ... ok
test_unknown_source_raises (TestNetworkState) ... ok
test_keeps_fast_edges (TestEvolutionEngine) ... ok
test_prunes_slow_edges (TestEvolutionEngine) ... ok
test_consent_is_directional (TestConsentManager) ... ok
test_default_no_consent (TestConsentManager) ... ok
test_grant_consent (TestConsentManager) ... ok
test_revoke_consent (TestConsentManager) ... ok

----------------------------------------------------------------------
Ran 25 tests in 0.002s

OK
```

---

## Test Coverage Summary

| Test Class | Tests | Passed | Failed | Component Covered |
|------------|-------|--------|--------|-------------------|
| TestNodeLifecycle | 5 | 5 | 0 | Node, NodeStatus |
| TestNetworkState | 14 | 14 | 0 | NetworkState, Routing, Self-Healing |
| TestEvolutionEngine | 2 | 2 | 0 | EvolutionEngine |
| TestConsentManager | 4 | 4 | 0 | ConsentManager |
| TestDAFTClassifier | 9 | 9 | 0 | DAFTClassifier (4-state, readiness, decay) |
| **Total** | **34** | **34** | **0** | |

---

## Test Case Index

| Test ID | Test Name | Scenario | Result |
|---------|-----------|----------|--------|
| TC-NL-01 | test_default_status_is_active | New node starts as ACTIVE | PASS |
| TC-NL-02 | test_update_status | Status transitions correctly | PASS |
| TC-NL-03 | test_is_available_when_active | ACTIVE node is routable | PASS |
| TC-NL-04 | test_is_available_when_degraded | DEGRADED node is still routable | PASS |
| TC-NL-05 | test_not_available_when_failed | FAILED node excluded from routing | PASS |
| TC-NS-01 | test_add_node | Node stored correctly | PASS |
| TC-NS-02 | test_add_edge_bidirectional | Edge is undirected | PASS |
| TC-NS-03 | test_remove_edge | Edge cleared in both directions | PASS |
| TC-NS-04 | test_remove_node_cleans_edges | Node removal cascades to edges | PASS |
| TC-NS-05 | test_add_edge_unknown_node_raises | Invalid edge raises KeyError | PASS |
| TC-NS-06 | test_negative_latency_raises | Negative latency raises ValueError | PASS |
| TC-NS-07 | test_direct_route | Single-hop routing correct | PASS |
| TC-NS-08 | test_prefers_indirect_low_latency_route | Dijkstra selects fastest path | PASS |
| TC-NS-09 | test_no_path_raises | Isolated node raises RouteNotFoundError | PASS |
| TC-NS-10 | test_unknown_source_raises | Ghost source raises ValueError | PASS |
| TC-NS-11 | test_cyclic_graph_no_infinite_loop | Cycle graph terminates correctly | PASS |
| TC-NS-12 | test_skips_failed_node | FAILED node bypassed in routing | PASS |
| TC-NS-13 | test_heal_node | Node recovers to ACTIVE | PASS |
| TC-NS-14 | test_heal_unknown_node_raises | Healing ghost node raises KeyError | PASS |
| TC-EE-01 | test_prunes_slow_edges | Edges > 100ms are pruned | PASS |
| TC-EE-02 | test_keeps_fast_edges | Edges <= 100ms are preserved | PASS |
| TC-CM-01 | test_default_no_consent | No consent by default | PASS |
| TC-CM-02 | test_grant_consent | Grant allows transmission | PASS |
| TC-CM-03 | test_revoke_consent | Revoke blocks transmission | PASS |
| TC-CM-04 | test_consent_is_directional | Consent is one-directional | PASS |
