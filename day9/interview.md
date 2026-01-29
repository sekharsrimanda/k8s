
---

## 📄 `Interview.md`

```md
# Day 9 — Resource Requests & Limits (Interview Q&A)

---

## Q1: What is the difference between resource requests and limits?
Requests define the minimum guaranteed resources used for scheduling.
Limits define the maximum resources enforced at runtime.

---

## Q2: Does the Kubernetes scheduler consider resource limits?
No. The scheduler only considers resource requests.

---

## Q3: What happens when a container exceeds its memory limit?
The container is terminated by the kernel with `OOMKilled` (exit code 137).

---

## Q4: What happens when a container exceeds its CPU limit?
The container is throttled; it is not killed.

---

## Q5: Who enforces resource limits in Kubernetes?
The kubelet enforces limits using container runtime and cgroups.

---

## Q6: Why are resource limits important in shared clusters?
They prevent noisy-neighbor issues and protect node stability.

---

## Q7: Why should requests be set even if limits exist?
Without requests, the scheduler may overcommit nodes, leading to instability.

---

## Q8: What is allocatable memory?
Allocatable memory is the portion of node memory available for Pods after reserving system resources.

---

## Q9: How do you debug an OOMKilled Pod?
Using:
- `kubectl describe pod`
- `kubectl get events`
- Checking termination reason and exit code

---

## Q10: Platform engineer best practice for limits?
Always size requests and limits based on node allocatable resources and workload behavior.

---
