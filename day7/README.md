# Day 7 - Kubernetes Probes (Hands-on)

## Goal
Practice Liveness, Readiness, and Startup probes and debug failures using describe/events/logs.

## What I Built
- Namespace: pe-day7
- Deployment: web (nginx) with readinessProbe, livenessProbe, startupProbe
- Service: web-svc (ClusterIP)

## Commands Used
kubectl apply -f 01-namespace.yaml
kubectl apply -f 02-deploy.yaml
kubectl apply -f 03-service.yaml

kubectl -n pe-day7 get pods -o wide
kubectl -n pe-day7 describe pod -l app=web
kubectl -n pe-day7 get events --sort-by=.metadata.creationTimestamp

## Break & Fix
### Break 1 (Readiness)
- Changed readinessProbe path from `/` to `/wrong`
- Result: Pod became NotReady (service stopped routing traffic)

### Fix
- Reverted readinessProbe path back to `/`
- Applied YAML again

## Debug Commands
kubectl -n pe-day7 describe pod <pod-name>
kubectl -n pe-day7 logs <pod-name>
kubectl -n pe-day7 get events --sort-by=.metadata.creationTimestamp
kubectl -n pe-day7 get pods -o wide
