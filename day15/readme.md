# Day 16 — Taints & Tolerations (Hands-on Lab)

## Objective
Understand how taints prevent Pod scheduling and how tolerations allow Pods to be scheduled on tainted nodes.

## Steps
1. Apply a NoSchedule taint to a node
2. Create a Pod without toleration (expected Pending)
3. Debug using kubectl describe
4. Create a Pod with toleration (expected Running)

## Key Learning
- Taints repel Pods
- Tolerations grant permission
- Tolerations do not force scheduling
