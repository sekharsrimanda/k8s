# Day 10 — Kubernetes Health Probes
## Platform Engineering — Hands-on Lab

---

## Goal
Understand and implement Kubernetes health probes:
- Liveness Probe
- Readiness Probe
- Startup Probe

Learn how probes affect:
- Pod restarts
- Service traffic routing
- Application availability

---

## What I Built
- Namespace: `day10`
- Deployment running `nginx:alpine`
- Configured:
  - Liveness probe
  - Readiness probe
  - Startup probe
- Service exposing the application
- Intentionally misconfigured readiness probe to simulate failure
- Debugged and fixed the issue declaratively

---

## Commands Used
```bash
kubectl apply -f 01-namespace.yaml
kubectl apply -f 02-deployment.yaml
kubectl apply -f 03-service.yaml

kubectl get pods -n day10
kubectl describe pod <pod-name> -n day10
kubectl get events -n day10 --sort-by=.metadata.creationTimestamp
kubectl get endpoints probe-service -n day10
kubectl logs <pod-name> -n day10
Break & Fix
Break
Readiness probe configured with invalid path /wrong

Pods were running but marked NotReady

Service had no endpoints

Observation
kubectl describe pod showed:

Readiness probe failed: HTTP probe failed with statuscode: 404

Pods were not added to Service endpoints

Fix
Corrected readiness probe path to /

Re-applied Deployment YAML

Pods transitioned to Ready

Service endpoints populated successfully

Debug Commands
kubectl describe pod <pod-name> -n day10
kubectl get events -n day10
kubectl get endpoints probe-service -n day10
kubectl logs <pod-name> -n day10
Key Learnings
Liveness controls container restarts

Readiness controls traffic routing

Startup protects slow-starting applications

A Pod can be Running but NotReady

Kubernetes relies entirely on probes for application health

Status
✔ Probes implemented
✔ Break & fix completed
✔ Debugging performed using describe/events/logs
✔ Declarative approach followed

