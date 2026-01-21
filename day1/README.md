# Day 01 - Kubernetes Deployment + Service (Killercoda)

## What I built
- Namespace: pe-lab
- Deployment: pe-web (nginx, replicas=2)
- Service: pe-web-svc (ClusterIP)

## Test
kubectl -n pe-lab run test --image=curlimages/curl -it --rm --restart=Never -- curl -s pe-web-svc

## Break & Fix
- Broke image: nginx -> nginxx
- Error: ErrImagePull / ImagePullBackOff
- Debug:
  - kubectl describe pod
  - kubectl get events
- Fix: set image back to nginx

## Files
- namespace-pe-lab.yaml
- deployment-pe-web.yaml
- service-pe-web-svc.yaml
