# Day 8 — ConfigMaps & Secrets

## Goal
Understand how Kubernetes externalizes configuration and sensitive data using
ConfigMaps and Secrets, and how misconfiguration causes Pod failures.

## What I Built
- Namespace: pe-day8
- ConfigMap: app-config
- Secret: app-secret
- Deployment consuming ConfigMap and Secret via environment variables
- Verified correct behavior after fixing misconfiguration

## Commands Used
kubectl apply -f 01-namespace.yaml
kubectl apply -f 02-configmap.yaml
kubectl apply -f 03-secret.yaml
kubectl apply -f 04-deployment.yaml

kubectl -n pe-day8 get pods
kubectl -n pe-day8 describe pod <pod-name>
kubectl -n pe-day8 get events --sort-by=.metadata.creationTimestamp
kubectl -n pe-day8 logs <pod-name>

## Break & Fix
### Break
- Used an incorrect key name (`DB_PASSWORD_WRONG`) in the Deployment.
- Pod failed with `CrashLoopBackOff`.

### Fix
- Corrected the Secret key reference to `DB_PASSWORD`.
- Re-applied Deployment YAML.
- Pod became `Running`.

## Debug Commands
kubectl -n pe-day8 describe pod <pod-name>
kubectl -n pe-day8 get events --sort-by=.metadata.creationTimestamp
kubectl -n pe-day8 logs <pod-name>
