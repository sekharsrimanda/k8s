# Day-02: Kubernetes ConfigMap + Secret (Hands-on)

## Goal
Practice using **ConfigMap** and **Secret** in a Kubernetes Deployment and verify environment variables inside the Pod.

---

## What I Built
- Namespace: `pe-day2`
- ConfigMap: `app-config`
- Secret: `app-secret`
- Deployment: `env-demo` (nginx) using ConfigMap + Secret as env variables

---

## Commands Used

### 1) Create Namespace
```bash
kubectl create ns pe-day2

##Create ConfigMap:

kubectl -n pe-day2 create configmap app-config \
  --from-literal=APP_NAME=platform-engineering \
  --from-literal=ENV=dev

##Create Secret:

kubectl -n pe-day2 create secret generic app-secret \
  --from-literal=DB_USER=admin \
  --from-literal=DB_PASS=Pass@123

##Apply Deployment YAML:

kubectl apply -f app.yaml
kubectl -n pe-day2 get pods

##Verify env variables inside Pod:

POD=$(kubectl -n pe-day2 get pod -o name | head -n 1)
kubectl -n pe-day2 exec -it ${POD} -- env | grep -E "APP_NAME|ENV|DB_USER|DB_PASS"

##Break & Fix (Mandatory Practice)
##Break: Delete ConfigMap and restart Pod

kubectl -n pe-day2 delete configmap app-config
kubectl -n pe-day2 delete pod -l app=env-demo
kubectl -n pe-day2 get pods


##Debug Commands

kubectl -n pe-day2 describe pod $(kubectl -n pe-day2 get pod -o name | head -n 1)
kubectl -n pe-day2 get events --sort-by=.metadata.creationTimestamp | tail -20


##Fix: Recreate ConfigMap and restart Pod

kubectl -n pe-day2 create configmap app-config \
  --from-literal=APP_NAME=platform-engineering \
  --from-literal=ENV=dev

kubectl -n pe-day2 delete pod -l app=env-demo
kubectl -n pe-day2 get pods


