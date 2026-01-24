✅ Day-04: Kubernetes Service Troubleshooting (Selector + Endpoints + TargetPort)
Goal

Practice troubleshooting a Kubernetes Service by checking selectors, endpoints, targetPort, and verifying connectivity using a curl pod.

What I Built

Namespace: pe-day4

Deployment: nginx-deployment (3 replicas)

Service: test-service (ClusterIP)

Debug Pod: curltest (curlimages/curl)

Commands Used
1) Create Namespace
kubectl create ns pe-day4

2) Create Deployment (nginx)

Save as: deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: pe-day4
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80


Apply:

kubectl apply -f deployment.yaml
kubectl -n pe-day4 get deploy
kubectl -n pe-day4 get pods -o wide

3) Create Service (ClusterIP)

Save as: service.yaml

apiVersion: v1
kind: Service
metadata:
  name: test-service
  namespace: pe-day4
spec:
  type: ClusterIP
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80


Apply + verify:

kubectl apply -f service.yaml
kubectl -n pe-day4 get svc -o wide
kubectl -n pe-day4 describe svc test-service

4) Verify Endpoints (Service → Pod mapping)
kubectl -n pe-day4 get endpoints test-service

5) Test Service Reachability (Inside Cluster)
kubectl -n pe-day4 run curltest --image=curlimages/curl -it --rm --restart=Never -- \
curl -s http://test-service | head

Break & Fix (Mandatory Practice)
Break: Change Service selector to wrong label

Edit service.yaml like below:

selector:
  app: wrong


Apply:

kubectl apply -f service.yaml
kubectl -n pe-day4 get endpoints test-service


Expected:

Endpoints should show <none> ❌

Debug Commands
kubectl -n pe-day4 describe svc test-service
kubectl -n pe-day4 get endpoints test-service
kubectl -n pe-day4 get pods --show-labels
kubectl -n pe-day4 get events --sort-by=.metadata.creationTimestamp | tail -20
kubectl -n pe-day4 describe pod $(kubectl -n pe-day4 get pod -o name | head -n 1)
kubectl -n pe-day4 logs $(kubectl -n pe-day4 get pod -o name | head -n 1)

Fix: Correct selector and re-apply

Fix service.yaml selector:

selector:
  app: nginx


Apply again:

kubectl apply -f service.yaml
kubectl -n pe-day4 get endpoints test-service
kubectl -n pe-day4 run curltest --image=curlimages/curl -it --rm --restart=Never -- \
curl -s http://test-service | head


Expected:

Endpoints show Pod IPs ✅

Curl returns nginx HTML ✅
