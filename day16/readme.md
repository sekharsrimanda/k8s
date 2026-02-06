# Day 17 — Node Affinity & Pod Anti‑Affinity (Hands-on)

## Objective
- Use node labels with required node affinity
- Intentionally break scheduling with a non-matching label
- Fix using correct affinity rules
- Enforce HA using pod anti-affinity

## What Happened
- Required affinity with non-existent label caused Pending
- Scheduler events showed unsatisfied affinity
- Correct label fixed scheduling
- Anti-affinity spread replicas across nodes
