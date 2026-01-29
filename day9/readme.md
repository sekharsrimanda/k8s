# Day 9 — Resource Requests & Limits (CPU / Memory)
## Platform Engineering — Hands-on Lab

---

## Goal
Understand how Kubernetes uses:
- **Resource requests** for scheduling
- **Resource limits** for runtime enforcement

Safely reproduce an **OOMKilled** scenario in a constrained environment
(Killercoda: 1 master + 1 worker) and fix it declaratively.

---

## Environment
- Platform: Killercoda
- Nodes: 1 control-plane + 1 worker
- Worker allocatable memory: ~1.8 Gi
- Worker allocatable CPU: 1 core

---

## What I Built
- Namespace: `day9`
- Deployment running `polinux/stress`
- Configured:
  - CPU & memory **requests**
  - CPU & memory **limits**
- Intentionally triggered **OOMKilled**
- Debugged using describe & events
- Fixed by increasing memory limit

---

## Deployment Configuration (Summary)
- Application tries to allocate **200Mi**
- Initial memory limit set to **120Mi**
- Result: Container killed by kernel (OOMKilled)
- Fixed by increasing limit to **250Mi**

---

## Commands Used
```bash
kubectl apply -f 01-namespace.yaml
kubectl apply -f 02-deployment.yaml

kubectl get pods -n day9
kubectl describe pod <pod-name> -n day9
kubectl get events -n day9 --sort-by=.metadata.creationTimestamp
kubectl logs <pod-name> -n day9
Break & Fix
Break
Memory limit set lower than application memory usage

Container exceeded memory limit

Pod entered CrashLoopBackOff

Termination reason: OOMKilled (Exit Code 137)

Fix
Increased memory limit above application usage

Re-applied Deployment YAML

Pod transitioned to Running

Debug Commands
kubectl describe pod <pod-name> -n day9
kubectl get events -n day9
kubectl logs <pod-name> -n day9
Key Learnings
Scheduler uses requests, not limits

Memory limits are enforced by the kernel

Exceeding memory limit results in OOMKilled

CPU limits cause throttling, not restarts

Resource sizing must consider node allocatable, not capacity

Status
✔ OOMKilled observed
✔ Debugged using describe/events/logs
✔ Fixed declaratively
✔ Safe for low-resource environments

