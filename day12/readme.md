# Day 12 — Horizontal Pod Autoscaler (HPA) Hands-on

## Goal
Understand and implement Kubernetes Horizontal Pod Autoscaler (HPA) using CPU-based autoscaling with metrics-server.

---

## What I Built
- Namespace: pe-day12-hpa
- Deployment: hpa-app (CPU constrained)
- Service: hpa-svc (ClusterIP)
- HPA: hpa-demo (CPU utilization based)

---

## Prerequisites
- Kubernetes cluster
- Metrics Server installed and running

---

## Files Created
- namespace.yaml
- deploy.yaml
- svc.yaml
- hpa.yaml

---

## Apply Order
```bash
kubectl apply -f namespace.yaml
kubectl apply -f deploy.yaml
kubectl apply -f svc.yaml
kubectl apply -f hpa.yaml
