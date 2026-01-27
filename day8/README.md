# Day 8 — ConfigMaps & Secrets
## Platform Engineering — Notes

---

## Goal
Understand how Kubernetes externalizes configuration and sensitive data using
ConfigMaps and Secrets, and how misconfiguration causes Pod failures.

---

## What I Built
- Namespace: `pe-day8`
- ConfigMap: `app-config`
- Secret: `app-secret`
- Deployment consuming:
  - ConfigMap via environment variables
  - Secret via environment variables
- Broke the Deployment using a wrong key
- Debugged and fixed the issue using events, describe, and logs

---

## Commands Used
```bash
kubectl apply -f 01-namespace.yaml
kubectl apply -f 02-configmap.yaml
kubectl apply -f 03-secret.yaml
kubectl apply -f 04-deployment.yaml

kubectl -n pe-day8 get pods
kubectl -n pe-day8 describe pod <pod-name>
kubectl -n pe-day8 get events --sort-by=.metadata.creationTimestamp
kubectl -n pe-day8 logs <pod-name>
kubectl -n pe-day8 get cm,secret
Break & Fix
Break

Used a wrong key name (DB_PASSWORD_WRONG) in secretKeyRef

Pod failed to start and entered CrashLoopBackOff

Observation

Events showed missing Secret key

Container failed during startup

Fix

Corrected the key name to DB_PASSWORD

Re-applied Deployment YAML

Pod became Running

Debug Commands
kubectl describe pod <pod-name>
kubectl get events --sort-by=.metadata.creationTimestamp
kubectl logs <pod-name>
kubectl get cm,secret

Fix (What Solved the Issue)

Ensured ConfigMap and Secret keys matched exactly

Verified existence of keys before Pod startup

Re-applied YAML and validated Pod status

Definitions

ConfigMap
Stores non-sensitive configuration data (key–value pairs) separately from application code.

Secret
Stores sensitive data such as passwords, tokens, and certificates. Data is base64-encoded by default.

env / envFrom
Mechanism to inject ConfigMap or Secret values into containers as environment variables.

CrashLoopBackOff
Pod state indicating that the container repeatedly fails and restarts.

Opaque Secret
Default Secret type used to store arbitrary key–value pairs.

Externalized Configuration
Practice of keeping configuration outside application code to allow environment-specific behavior.

5 Key Commands
kubectl get cm,secret
kubectl describe pod <pod-name>
kubectl get events --sort-by=.metadata.creationTimestamp
kubectl logs <pod-name>
kubectl get pods

3 Common Errors + Fixes

Pod fails due to missing key

Cause: Wrong key name in ConfigMap/Secret

Fix: Match key names exactly

Config change not reflected

Cause: Pods don’t auto-reload env vars

Fix: Restart Pods

Secrets committed in plain text

Cause: Poor security practice

Fix: Use encrypted secret management or external secret tools

Interview Q&A

Q1. Difference between ConfigMap and Secret?
ConfigMap stores non-sensitive data, Secret stores sensitive data.

Q2. Are Secrets encrypted in Kubernetes?
By default, they are base64-encoded, not encrypted (encryption at rest must be enabled).

Q3. How can ConfigMaps be consumed?
As environment variables or mounted files.

Q4. Do Pods automatically reload ConfigMap changes?
No, Pods must be restarted.

Q5. How do you debug ConfigMap/Secret failures?
Using kubectl describe pod, events, and logs.

Flash Points

Config ≠ code

Secret ≠ encrypted by default

Wrong key = Pod failure

Env vars need Pod restart

Events reveal the root cause
