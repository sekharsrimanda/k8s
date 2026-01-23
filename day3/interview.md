✅ DAY-3 — Ingress + Ingress Controller + NodePort Testing + Rewrite Fix
📌 Definitions (Day-3 Concepts)
1) Ingress

Definition: Ingress is a Kubernetes resource that defines HTTP/HTTPS routing rules to send traffic to Services.

2) Ingress Controller

Definition: An Ingress Controller is the component (like NGINX) that runs in the cluster and actually implements Ingress rules.

3) IngressClass

Definition: IngressClass defines which controller should handle an Ingress resource (example: nginx).

4) Path-based Routing

Definition: Routing requests to different services based on URL paths like /app1 and /app2.

5) NodePort

Definition: NodePort exposes a service on a port of every node, allowing external access using nodeIP:nodePort.

6) 404 Not Found in Ingress

Definition: A 404 response indicates the request did not match a route correctly OR the backend app doesn’t support that path.

7) Rewrite Target Annotation

Definition: Rewrite target rewrites the request path (example /app1) to / before sending to backend service.

8) Endpoints

Definition: Endpoints represent the actual Pod IPs behind a Service. If endpoints are empty, service has no pods matched.

🎯 Day-3 Interview Questions & Answers
Q1) What is Ingress used for?

✅ Answer:
Ingress exposes HTTP/HTTPS routes from outside the cluster to internal services.

Q2) Why do we need an Ingress Controller?

✅ Answer:
Ingress is only routing rules. The controller (nginx/traefik) makes it work by handling traffic.

Q3) Ingress vs Service LoadBalancer?

✅ Answer:
LoadBalancer exposes one service. Ingress can route to multiple services using paths/hosts.

Q4) What is IngressClassName?

✅ Answer:
It specifies which ingress controller should handle the ingress rules.

Q5) How do you debug Ingress routing issues?

✅ Answer:
Check:

kubectl describe ingress

service endpoints

ingress controller pods/logs

events

Q6) Why did we get 404 when hitting /app1?

✅ Answer:
Because backend nginx does not serve /app1 path. Ingress route matched, but backend returned 404.

Q7) How did we fix the 404?

✅ Answer:
By adding rewrite annotation:

nginx.ingress.kubernetes.io/rewrite-target=/

Q8) What is the purpose of Endpoints in Kubernetes?

✅ Answer:
Endpoints show which pods are behind a service. It confirms service-to-pod mapping is correct.
