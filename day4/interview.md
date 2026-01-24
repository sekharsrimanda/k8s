📌 Definitions (Day-4 Concepts)
Service

Definition: Service provides a stable DNS name and virtual IP to access Pods, and load-balances traffic.

Selector

Definition: Selector is used by Service to match Pods using labels.

Endpoints

Definition: Endpoints show the actual Pod IPs behind a Service. If endpoints are empty, service has no backend pods.

port vs targetPort

Definition:

port = Service port exposed to clients

targetPort = container port where traffic is forwarded

kube-proxy

Definition: kube-proxy routes Service traffic to the correct Pod endpoints using iptables/ipvs rules.

🎯 Day-4 Interview Questions & Answers
Q1) What is a Kubernetes Service used for?

✅ Answer: To provide stable access to Pods using DNS/ClusterIP and load-balance traffic.

Q2) What does it mean if endpoints show <none>?

✅ Answer: Service has no backend Pods (selector mismatch or Pods not Ready).

Q3) How does a Service select Pods?

✅ Answer: Using spec.selector which must match Pod labels.

Q4) port vs targetPort difference?

✅ Answer: port is service port; targetPort is container port where traffic is sent.

Q5) How to troubleshoot a Service not working?

✅ Answer:

kubectl describe svc

kubectl get endpoints

kubectl get pods --show-labels

curl test pod inside cluster
