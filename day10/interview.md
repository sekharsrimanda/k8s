
---

## 📄 `Interview.md`

```md
# Day 10 — Kubernetes Health Probes (Interview Q&A)

---

## Q1: What is a liveness probe?
A liveness probe checks if a container is still running correctly. If it fails, Kubernetes restarts the container.

---

## Q2: What is a readiness probe?
A readiness probe determines whether a Pod is ready to receive traffic. If it fails, the Pod is removed from Service endpoints but not restarted.

---

## Q3: What problem does a startup probe solve?
Startup probes protect slow-starting applications from being killed by liveness probes during initialization.

---

## Q4: Can a Pod be running but not ready?
Yes. A Pod can be in `Running` state while failing its readiness probe and therefore not receiving traffic.

---

## Q5: What happens if a readiness probe keeps failing?
The Pod stays running but is excluded from Service endpoints.

---

## Q6: Who executes health probes in Kubernetes?
The kubelet running on the node executes liveness, readiness, and startup probes.

---

## Q7: When should you avoid liveness probes?
When application restarts can cause data corruption or make recovery worse.

---

## Q8: What are common probe types?
- httpGet
- tcpSocket
- exec

---

## Q9: How do probes help platform teams?
They improve reliability, enable safe rollouts, and prevent unhealthy Pods from serving traffic.

---

## Q10: How do you debug probe failures?
Using:
- `kubectl describe pod`
- `kubectl get events`
- Container logs

---
