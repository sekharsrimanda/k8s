# Day 9 — Resource Requests, Limits, ResourceQuota & LimitRange
## Platform Engineering — Resource Management Fundamentals

---

## Why this topic matters (Context)

In Kubernetes, resource management is **critical for stability, fairness, and multi-tenancy**.
Without proper control:
- One Pod can consume all CPU or memory
- Nodes can crash
- Other applications can starve

Kubernetes solves this using:
- Resource Requests
- Resource Limits
- ResourceQuota
- LimitRange

Each solves a **different problem** and works at a **different level**.

---

# Resource Requests

## Definition
Resource requests specify the **minimum amount of CPU and memory** that a container requires to run.

## Use
Resource requests are used to:
- Help the scheduler decide **where to place a Pod**
- Guarantee a minimum amount of resources to a container
- Enable predictable scheduling and autoscaling

## How resource requests work
1. A Pod is created with resource requests
2. The scheduler checks each node’s **allocatable resources**
3. The Pod is scheduled only on nodes that can satisfy the requests
4. Once scheduled, the container is **guaranteed** those resources

Important:
- Requests are used **only for scheduling**
- Requests are **not enforced as hard limits**

---

## CPU Requests
- Measured in cores
- `100m` = 0.1 CPU core

If CPU usage exceeds request:
- Container can still use more CPU (if available)

---

## Memory Requests
- Measured in bytes (Mi, Gi)

If memory usage exceeds request:
- Container continues running
- No action is taken by the kernel

---

## Key takeaway
> **Requests answer: “Where can this Pod be scheduled?”**

---

# Resource Limits

## Definition
Resource limits specify the **maximum amount of CPU and memory** a container is allowed to consume.

## Use
Resource limits are used to:
- Prevent resource abuse
- Protect node stability
- Avoid noisy-neighbor problems

## How resource limits work
1. Kubernetes passes limits to the container runtime
2. The runtime configures **cgroups**
3. The Linux kernel enforces the limits

Kubernetes does **not** kill or throttle containers directly.

---

## CPU Limits
- CPU is **throttled**
- Container is slowed down
- Container is **not killed**

Why:
CPU is a compressible resource.

---

## Memory Limits
- Memory is **not compressible**
- Exceeding memory limit triggers **OOMKilled**
- Exit code: **137**

This is enforced by the **kernel**, not Kubernetes.

---

## Key takeaway
> **Limits answer: “How much can this container use at most?”**

---

# Relationship Between Requests and Limits

| Aspect | Requests | Limits |
|-----|--------|-------|
| Purpose | Scheduling | Enforcement |
| Used by | Scheduler | Kernel (via cgroups) |
| CPU behavior | Soft | Throttled |
| Memory behavior | Soft | OOMKilled |
| Mandatory | Recommended | Recommended |

---

# ResourceQuota

## Definition
A ResourceQuota is a **namespace-level policy** that limits the **total amount of resources** that all objects in a namespace can consume.

## Use
ResourceQuota is used to:
- Enforce fairness in multi-tenant clusters
- Prevent one namespace from consuming all resources
- Control object counts (Pods, Services, PVCs)

## How ResourceQuota works
1. Admin creates a ResourceQuota in a namespace
2. Kubernetes tracks **aggregate usage** in that namespace
3. When a new object is created:
   - API Server checks quota usage
   - If quota would be exceeded → request is **rejected**
4. Pod is **never created**

ResourceQuota is enforced at the **API Server level**, before scheduling.

---

## What ResourceQuota can limit
- `requests.cpu`
- `requests.memory`
- `limits.cpu`
- `limits.memory`
- Number of Pods
- Number of Services
- PVC storage

---

## Key takeaway
> **ResourceQuota answers: “How much can this namespace use in total?”**

---

# LimitRange

## Definition
A LimitRange is a **namespace-level policy** that defines **minimum, maximum, and default** resource values **per Pod or per container**.

## Use
LimitRange is used to:
- Enforce sane defaults
- Prevent extreme values
- Ensure all Pods define requests and limits

## How LimitRange works
1. Admin creates a LimitRange in a namespace
2. When a Pod is created:
   - If requests/limits are missing → defaults are applied
   - If values are outside min/max → request is **rejected**
3. Pod is created only if it complies

LimitRange works at the **individual workload level**, not aggregate.

---

## What LimitRange can control
- CPU requests & limits
- Memory requests & limits
- PVC size (min/max)

---

## Key takeaway
> **LimitRange answers: “What values are allowed per Pod/container?”**

---

# ResourceQuota vs LimitRange (Critical Interview Topic)

| Aspect | ResourceQuota | LimitRange |
|-----|--------------|-----------|
| Scope | Namespace (total) | Pod / Container |
| Purpose | Fairness | Guardrails |
| Enforced by | API Server | API Server |
| Works on | Aggregate usage | Individual specs |
| Defaults applied | ❌ No | ✅ Yes |
| Prevents | Namespace overuse | Bad Pod configs |

---

# How They Work Together (Real Clusters)

Typical production setup:
1. **LimitRange** ensures every Pod has valid requests/limits
2. **ResourceQuota** ensures namespace stays within budget
3. Scheduler places Pods using requests
4. Kernel enforces limits via cgroups

---

# Common Failure Scenarios (Interview-Ready)

## Pod rejected immediately
- Cause: ResourceQuota exceeded or LimitRange violated

## Pod scheduled but slow
- Cause: CPU throttling due to limits

## Pod crashes repeatedly
- Cause: Memory limit too low → OOMKilled

---

# Key Interview Takeaways

- Requests are for scheduling
- Limits are enforced by the kernel
- ResourceQuota protects the namespace
- LimitRange protects the cluster from bad configs
- Kubernetes itself does not enforce resource usage

---

## One-line Interview Answer
> “Resource requests guide scheduling, resource limits enforce usage via cgroups, ResourceQuota limits total namespace consumption, and LimitRange enforces per-Pod defaults and boundaries.”

---

## Status
- Concepts understood deeply ✔
- Labs validated behavior ✔
- Ready for HPA & autoscaling ✔

---
