# Weekend Revision: Ingress — Interview Q&A

Q1) What is Ingress used for?
Answer: Ingress exposes HTTP/HTTPS routes from outside the cluster to internal services.

Q2) Why do we need an Ingress Controller?
Answer: Ingress is only routing rules. Controller (nginx/traefik) makes it work.

Q3) Ingress vs Service LoadBalancer?
Answer: LoadBalancer exposes one service. Ingress routes to multiple services using paths/hosts.

Q4) Why 404 happens in Ingress?
Answer: Backend app may not support the path (/app1). Route matched but backend returned 404.

Q5) How to fix 404 for path-based routing?
Answer: Use rewrite annotation:
nginx.ingress.kubernetes.io/rewrite-target: /

Q6) How to debug ingress issues?
Answer: Check ingress describe, service endpoints, controller pods/logs, events.
