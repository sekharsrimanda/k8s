# Day 18 — DaemonSets (Platform Engineering)

## Goal
Understand and implement **DaemonSets**, which ensure that a specific Pod runs on **every eligible node** in a Kubernetes cluster.

DaemonSets are primarily used for **node-level platform components** such as logging, monitoring, networking, and security agents.

---

## What I Built
- A DaemonSet that deploys a lightweight agent on each node
- Verified automatic Pod creation per node
- Intentionally broke scheduling using an invalid `nodeSelector`
- Debugged the issue using `kubectl describe` and cluster events
- Fixed the DaemonSet declaratively

---

## Hands-on Summary

### Broken Scenario
- DaemonSet used a `nodeSelector` that matched no nodes
- Result: **0 Pods created**
- DaemonSet existed, but no eligible nodes were found

### Fixed Scenario
- Removed the invalid nodeSelector
- Re-applied DaemonSet
- Result: **1 Pod per node**, all in `Running` state

---

## Key Learnings
- DaemonSets scale by **node count**, not replicas
- Node constraints (labels, affinity, taints) fully apply
- Debugging focuses on the **DaemonSet controller**, not individual Pods
- DaemonSets are critical for platform-level workloads

---

## Validation Commands
```bash
kubectl get daemonsets
kubectl describe daemonset <daemonset-name>
kubectl get pods -o wide
kubectl get nodes
