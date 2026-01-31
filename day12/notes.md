# Day 12 — Horizontal Pod Autoscaler (HPA)

## Definition
Horizontal Pod Autoscaler (HPA) is a Kubernetes controller that **automatically adjusts the number of pod replicas** in a workload (Deployment/ReplicaSet/StatefulSet) based on **observed resource usage metrics**, most commonly CPU utilization.

---

## Why HPA Exists
Static replica counts cannot handle:
- Sudden traffic spikes
- Unpredictable workloads
- Cost optimization needs

HPA allows Kubernetes applications to:
- Scale **out** under load
- Scale **in** when load reduces
- Maintain performance while optimizing resources

---

## What HPA Scales (Important)
- HPA scales **pods**, not nodes
- Node scaling is handled by **Cluster Autoscaler**
- HPA works at the **application layer**

---

## HPA Architecture (How it Works)

1. Application receives traffic
2. Pod CPU usage increases
3. Kubelet reports metrics
4. Metrics Server aggregates metrics
5. HPA controller queries metrics API
6. HPA calculates average utilization
7. Replica count is increased or decreased

---

## Metrics Server (Critical Dependency)

### Definition
Metrics Server is a cluster-wide aggregator that collects **CPU and memory usage** from kubelets and exposes them via the `metrics.k8s.io` API.

### Why Metrics Server is Mandatory
- HPA uses metrics API to calculate utilization
- `kubectl top` depends on metrics-server
- Without metrics-server, HPA **cannot function**

---

## CPU Utilization Formula (Exam Favorite)

