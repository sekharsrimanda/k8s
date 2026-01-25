# Weekend Revision: Ingress Deep Practice (Declarative YAML)

## Goal
Revise Ingress by doing full hands-on using declarative YAML: 2 Deployments + 2 Services + 1 Ingress with path routing and rewrite fix.

## What I Built
- Namespace: pe-weekend-ingress
- Deployments: app1, app2
- Services: app1-svc, app2-svc
- Ingress: lab-ingress

## Commands Used
1) Apply YAMLs
kubectl apply -f namespace.yaml
kubectl apply -f app1-deploy.yaml
kubectl apply -f app1-svc.yaml
kubectl apply -f app2-deploy.yaml
kubectl apply -f app2-svc.yaml
kubectl apply -f ingress.yaml

2) Verify
kubectl -n pe-weekend-ingress get pods -o wide
kubectl -n pe-weekend-ingress get svc -o wide
kubectl -n pe-weekend-ingress get endpoints app1-svc
kubectl -n pe-weekend-ingress get endpoints app2-svc
kubectl -n pe-weekend-ingress get ingress
kubectl -n pe-weekend-ingress describe ingress lab-ingress

3) Test externally (example)
curl http://<NODE-IP>:<NODEPORT>/app1
curl http://<NODE-IP>:<NODEPORT>/app2

## Break & Fix (Mandatory Practice)
Break: remove rewrite-target annotation from ingress.yaml → 404
Fix: add rewrite annotation back:
nginx.ingress.kubernetes.io/rewrite-target: /

## Debug Commands
kubectl -n pe-weekend-ingress describe ingress lab-ingress
kubectl -n pe-weekend-ingress get endpoints app1-svc
kubectl -n pe-weekend-ingress get endpoints app2-svc
kubectl get pods -A | grep ingress
kubectl -n <ingress-namespace> logs <controller-pod>
