✅ DAY-1 — Kubernetes Deployment + Service (ClusterIP) + Debugging
📌 Definitions (Day-1 Concepts)
1) Kubernetes Namespace

Definition: A Namespace is a logical partition in Kubernetes used to isolate and organize resources within the same cluster.

2) Pod

Definition: A Pod is the smallest deployable unit in Kubernetes that runs one or more containers sharing the same network and storage.

3) Deployment

Definition: A Deployment is a controller that manages Pods via ReplicaSets, ensuring the desired replicas are running and supporting rolling updates/rollback.

4) ReplicaSet

Definition: A ReplicaSet ensures a specified number of pod replicas are running at all times.

5) Service

Definition: A Service provides a stable network endpoint (DNS/IP) to access a set of Pods and load balances traffic across them.

6) ClusterIP Service

Definition: ClusterIP is the default service type that exposes an application only within the cluster.

7) Labels & Selectors

Definition: Labels are key-value tags on objects; selectors are used by Services/Deployments to match and manage the right Pods.

8) Events (Kubernetes Events)

Definition: Events are system messages that show what happened to an object (scheduling, pulling image, failures, etc.).

9) kubectl describe

Definition: kubectl describe shows detailed info + events for a Kubernetes object (best for debugging).

10) kubectl logs

Definition: kubectl logs displays container logs inside a Pod for troubleshooting application issues.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

🎯 Day-1 Interview Questions & Answers
Q1) What is the difference between Pod and Deployment?

✅ Answer:
A Pod runs containers, but it’s not self-healing. A Deployment manages Pods, ensures replicas, supports scaling and rolling updates.

Q2) Why do we need a Service if Pods are running?

✅ Answer:
Pod IPs change when pods restart. A Service provides a stable DNS/IP and load balances across pods.

Q3) What is ClusterIP?

✅ Answer:
ClusterIP exposes the service only inside the Kubernetes cluster and is used for internal communication.

Q4) How does a Service know which Pods to send traffic to?

✅ Answer:
Using selectors that match Pod labels.

Q5) What causes ImagePullBackOff?

✅ Answer:
Wrong image name/tag, registry access issues, network problems, or missing credentials.

Q6) How do you debug a failing Pod quickly?

✅ Answer:
Use this order:

kubectl describe pod

kubectl logs

kubectl get events

Q7) What is the purpose of ReplicaSet?

✅ Answer:
ReplicaSet ensures the required number of Pod replicas are always running.

Q8) What is “desired state” in Kubernetes?

✅ Answer:
The configuration you declare (replicas, image, etc.) and Kubernetes continuously works to maintain it.
