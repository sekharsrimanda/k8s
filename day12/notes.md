# Day 12 — HPA Notes (Platform Engineering)

## Key Concepts
- HPA scales pods, not nodes
- Requires metrics-server
- Works on average CPU utilization over time
- Not instant; reacts with delay

---

## HPA Flow
Client Load
→ Service
→ Pod CPU increases
→ Metrics Server collects data
→ HPA evaluates CPU %
→ Replica count increases

---

## Metrics Server
- Collects resource usage from kubelets
- Exposes metrics via metrics.k8s.io API
- Mandatory for HPA and kubectl top

### Install
```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
