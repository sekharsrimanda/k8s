# Day-03: Kubernetes Ingress Hands-on (Killercoda)

## Goal
Practice Kubernetes **Ingress + Ingress Controller** by routing traffic to two services:
- `/app1` → nginx
- `/app2` → http-echo

---

## What I Built
- Namespace: `ingress-lab`
- Deployments:
  - `app1-nginx` (nginx)
  - `app2-echo` (hashicorp/http-echo)
- Services:
  - `app1-svc` (ClusterIP)
  - `app2-svc` (ClusterIP)
- Ingress:
  - `lab-ingress` (IngressClass: nginx)
- Ingress Controller:
  - `ingress-nginx` (nginx ingress controller)

---

## Commands Used

### 1) Create Namespace
```bash
kubectl create ns ingress-lab
2) Deploy App1 (nginx) + Service
kubectl -n ingress-lab apply -f app1-deploy.yaml
kubectl -n ingress-lab apply -f app1-svc.yaml

3) Deploy App2 (http-echo) + Service
kubectl -n ingress-lab apply -f app2-deploy.yaml
kubectl -n ingress-lab apply -f app2-svc.yaml

4) Install Ingress Controller
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
kubectl get pods -n ingress-nginx

5) Create Ingress
kubectl -n ingress-lab apply -f ingress.yaml
kubectl -n ingress-lab get ingress
kubectl -n ingress-lab describe ingress lab-ingress

External Testing (NodePort)
Get NodePort
kubectl -n ingress-nginx get svc ingress-nginx-controller

Test from terminal
curl http://172.30.1.2:<NODEPORT>/app1
curl http://172.30.1.2:<NODEPORT>/app2

Issue Faced + Fix (Important Learning)
❌ Issue: Got 404 Not Found when hitting /app1

Example:

curl http://172.30.1.2:<NODEPORT>/app1


Output:

404 Not Found

✅ Reason

nginx app does not have /app1 path by default.

✅ Fix: Rewrite the path using Ingress annotation
kubectl -n ingress-lab annotate ingress lab-ingress nginx.ingress.kubernetes.io/rewrite-target=/ --overwrite


After this, routing works correctly.

Break & Fix (Mandatory Practice)
Break: Wrong backend service name in Ingress

Edited Ingress and changed:

app2-svc → app2-wrong

Debug Commands
kubectl -n ingress-lab describe ingress lab-ingress
kubectl -n ingress-lab get events --sort-by=.metadata.creationTimestamp | tail -20
kubectl -n ingress-lab get endpoints

Fix

Changed service name back to correct:

app2-svc

Files Saved

app1-deploy.yaml

app1-svc.yaml

app2-deploy.yaml

app2-svc.yaml

ingress.yaml

README.md

Key Learnings (Day-03)

Ingress routes HTTP/HTTPS traffic to services using path/host rules

Ingress rules work only when Ingress Controller is installed

404 errors can happen due to missing path support in backend apps

rewrite-target annotation helps route /app1 → /

Debugging tools:

kubectl describe ingress

kubectl get events

kubectl get endpoints
