# Day 14 — Vertical Pod Autoscaler (VPA)
## Platform Engineering — Resource Optimization & Autoscaling

---

## Why this topic matters (Context)

In Kubernetes, correct CPU and memory sizing is difficult:
- Over-requesting wastes cluster capacity
- Under-requesting causes throttling or OOMKilled
- Static sizing does not adapt to workload changes

**Vertical Pod Autoscaler (VPA)** solves this by **dynamically tuning resource requests (and optionally limits)** based on real usage over time.

This is a **platform-level feature**, not a developer convenience.

---

# Vertical Pod Autoscaler (VPA)

## Definition
Vertical Pod Autoscaler (VPA) is a Kubernetes autoscaling component that **recommends or automatically updates CPU and memory requests (and optionally limits)** for Pods based on historical resource usage.

---

## Use
VPA is used to:
- Optimize resource requests automatically
- Reduce waste from over-provisioning
- Prevent OOMKills caused by under-provisioning
- Improve scheduling efficiency in multi-tenant clusters

---

## What VPA scales (Important)
- ✅ CPU **requests**
- ✅ Memory **requests**
- ⚠️ CPU/memory **limits** (optional, risky)
- ❌ Number of Pods (that’s HPA)

---

## How VPA works (High-level flow)

1. Pods run normally in the cluster
2. Resource usage is collected over time
3. VPA analyzes historical usage patterns
4. VPA generates **recommended resource values**
5. Depending on mode:
   - Applies recommendations
   - Or only reports them

---

# VPA Architecture (Core Components)

VPA is **not built into Kubernetes**.  
It runs as **separate controllers**.

---

## 1️⃣ VPA Recommender

### Definition
A controller that **analyzes historical CPU and memory usage** and produces recommended values.

### Use
- Determines optimal resource requests
- Calculates:
  - Lower bound
  - Target
  - Upper bound

### How it works
- Reads metrics collected over time
- Uses statistical models
- Ignores short-lived spikes

---

## 2️⃣ VPA Updater

### Definition
A controller responsible for **evicting Pods** when resource updates are required.

### Use
- Enables VPA **Auto mode**
- Ensures Pods restart with new requests

### How it works
- Identifies Pods with outdated requests
- Evicts Pods safely
- New Pods are created with updated values

⚠️ Causes Pod restarts.

---

## 3️⃣ VPA Admission Controller

### Definition
A mutating admission webhook that **injects recommended resources** into Pods at creation time.

### Use
- Applies recommendations automatically
- Ensures new Pods start with optimized resources

### How it works
- Intercepts Pod creation requests
- Modifies resource requests before Pod is created

---

# VPA Modes (Critical for Interviews)

## Off
### Definition
Recommendation-only mode.

### Use
- Safe for production
- No Pod restarts

### How it works
- VPA calculates recommendations
- User manually applies changes

---

## Initial
### Definition
Applies recommendations **only at Pod creation time**.

### Use
- Balances safety and automation

### How it works
- Existing Pods unchanged
- New Pods get updated requests

---

## Auto
### Definition
Fully automated resource updates.

### Use
- Works best for batch jobs
- Risky for latency-sensitive workloads

### How it works
- VPA evicts Pods
- Pods restart with new requests

---

# VPA and Other Autoscalers

## VPA vs HPA

| Aspect | VPA | HPA |
|-----|----|----|
| Scales | Resources | Replicas |
| Based on | Historical usage | Current metrics |
| Pod restarts | Possible | No |
| Risk level | Medium | Low |

---

## VPA + HPA Rule (VERY IMPORTANT)

❌ Do NOT let both scale the **same resource**

Safe patterns:
- HPA on replicas + VPA on **memory**
- HPA on replicas + VPA in **Off mode**
- VPA recommendations + manual tuning

---

# Extensions Required for VPA Lab (VERY IMPORTANT)

## Why installation is required
VPA is **not a core Kubernetes API**.
It is implemented as a **Custom Resource**.

Without installation:
- Kubernetes does NOT recognize `VerticalPodAutoscaler`
- You get:
