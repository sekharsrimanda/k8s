✅ Day-05: Kubernetes Secrets (ENV + Volume Mount)
Goal

Practice creating Kubernetes Secrets and using them inside a Pod via:

Environment Variables

Volume Mount (files inside container)

What I Built

Namespace: pe-day5

Secret: app-secret

Pod 1: secret-env-pod (Secret as ENV)

Pod 2: secret-vol-pod (Secret as Volume files)

Commands Used
1) Create Namespace
kubectl create ns pe-day5

2) Create Secret (Generic)
kubectl -n pe-day5 create secret generic app-secret \
  --from-literal=DB_USER=admin \
  --from-literal=DB_PASS=Pass@123


Verify:

kubectl -n pe-day5 get secret
kubectl -n pe-day5 describe secret app-secret

3) Pod using Secret as ENV

Save as: pod-env.yaml

apiVersion: v1
kind: Pod
metadata:
  name: secret-env-pod
  namespace: pe-day5
spec:
  containers:
    - name: test
      image: busybox:1.36
      command: ["sh", "-c", "sleep 3600"]
      envFrom:
        - secretRef:
            name: app-secret


Apply + verify:

kubectl apply -f pod-env.yaml
kubectl -n pe-day5 get pod

kubectl -n pe-day5 exec -it secret-env-pod -- env | grep DB_


Expected:

DB_USER=admin

DB_PASS=Pass@123

4) Pod using Secret as Volume Mount

Save as: pod-vol.yaml

apiVersion: v1
kind: Pod
metadata:
  name: secret-vol-pod
  namespace: pe-day5
spec:
  containers:
    - name: test
      image: busybox:1.36
      command: ["sh", "-c", "sleep 3600"]
      volumeMounts:
        - name: secret-vol
          mountPath: /etc/secret
          readOnly: true
  volumes:
    - name: secret-vol
      secret:
        secretName: app-secret


Apply + verify:

kubectl apply -f pod-vol.yaml
kubectl -n pe-day5 get pod

kubectl -n pe-day5 exec -it secret-vol-pod -- ls -l /etc/secret
kubectl -n pe-day5 exec -it secret-vol-pod -- cat /etc/secret/DB_USER
kubectl -n pe-day5 exec -it secret-vol-pod -- cat /etc/secret/DB_PASS

Break & Fix (Mandatory Practice)
Break: Use wrong secret name in Pod YAML

Edit pod-env.yaml like:

name: wrong-secret


Apply:

kubectl apply -f pod-env.yaml
kubectl -n pe-day5 get pod

Debug Commands
kubectl -n pe-day5 describe pod secret-env-pod
kubectl -n pe-day5 get events --sort-by=.metadata.creationTimestamp | tail -20
kubectl -n pe-day5 get secret
kubectl -n pe-day5 describe secret app-secret

Fix: Correct secret name and re-apply

Fix pod-env.yaml back:

name: app-secret


Apply:

kubectl apply -f pod-env.yaml
kubectl -n pe-day5 get pod
kubectl -n pe-day5 exec -it secret-env-pod -- env | grep DB_
