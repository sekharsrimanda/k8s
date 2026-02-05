## 1‑Page Learning Notes

Affinity rules let you express placement intent for Pods.
Where taints/tolerations are exclusion-first (“who is NOT allowed”), affinity is preference/requirement-first (“where I WANT / MUST run”).

There are two scopes:

# 1) Node Affinity

Controls which nodes a Pod can (or prefers to) run on, based on node labels.

## requiredDuringSchedulingIgnoredDuringExecution

Hard rule

Pod will not schedule unless condition matches

## preferredDuringSchedulingIgnoredDuringExecution

Soft rule

Scheduler tries to honor, but may ignore if needed

## Key idea: Node Affinity replaces and extends nodeSelector with richer logic (AND/OR, operators).

# 2) Pod Affinity / Anti‑Affinity

Controls how Pods are placed relative to other Pods, based on labels + topology.

## Pod Affinity

“Place my Pod near other Pods”

Common for caching, data locality

## Pod Anti‑Affinity

“Do not place my Pod near other Pods”

## Critical for high availability

Topology key defines the boundary:

kubernetes.io/hostname → same node

topology.kubernetes.io/zone → same AZ

Hard vs Soft rules apply here as well.

## Why Affinity Exists (Real‑World)

Spread replicas across nodes/zones

Co-locate dependent services

Enforce HA without manual placement

Reduce blast radius of failures

## Affinity vs Taints (Mental Model)

Taints: Node says NO by default

Affinity: Pod says I WANT / I NEED

In production, they are often used together.

## Common Failure Modes (Interview‑Important)

Pod stuck Pending → required affinity not satisfiable

Over‑strict anti‑affinity → cluster cannot scale

Missing labels → rules never match

## 5 Key Commands
# View node labels (critical for node affinity)
kubectl get nodes --show-labels

# Describe a pod to see affinity evaluation
kubectl describe pod <pod-name>

# List pods with node placement
kubectl get pods -o wide

# Label a node (used by affinity rules)
kubectl label node <node-name> disktype=ssd

# Debug scheduling issues
kubectl get events --sort-by=.metadata.creationTimestamp

5 Interview Q&A

## Q1. NodeSelector vs Node Affinity?
NodeSelector is simple equality; Node Affinity supports complex expressions and soft rules.

## Q2. What happens if required node affinity cannot be satisfied?
The Pod stays in Pending state.

## Q3. Why use Pod Anti‑Affinity?
To spread replicas across nodes or zones for high availability.

## Q4. What is topologyKey?
It defines the boundary (node, zone, region) for affinity rules.

## Q5. Affinity vs Taints — which to use when?
Use taints to protect nodes; use affinity to guide Pod placement.

# Learning Resources
🎥 Direct Videos

Kubernetes Affinity & Anti‑Affinity (clear + practical):
https://www.youtube.com/watch?v=0Omvgd7Hg1Y

Scheduler internals (affinity explained):
https://www.youtube.com/watch?v=Yf1vXl5r1zU

# 📘 Official Documentation

Affinity and Anti‑Affinity — Kubernetes Official Docs
https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#affinity-and-anti-affinity

Node Labels & Selectors
https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/

Summary (Lock‑In)

Taints protect nodes.
Affinity guides Pods.
Anti‑affinity preserves availability.
