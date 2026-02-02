# Day 14 — Vertical Pod Autoscaler (VPA)

## Objective
Demonstrate VPA in recommendation-only mode and understand how it derives CPU and memory values.

## Components
- Deployment with fixed requests/limits
- VPA in Off mode

## Key Learnings
- VPA uses historical usage
- No Pod restarts in Off mode
- TargetRef must match workload exactly
