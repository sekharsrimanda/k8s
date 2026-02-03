# Day 15 — PodDisruptionBudgets (PDB)

## Objective
Protect application availability during voluntary disruptions.

## Components
- Deployment (3 replicas)
- PodDisruptionBudget with minAvailable

## Key Learnings
- PDBs control voluntary evictions only
- Budgets must align with replica count
- Overly strict PDBs can block node drains
