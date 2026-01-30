# Day 11 — Linux Namespaces & cgroups  
## Platform Engineering — Core Container Fundamentals

---

## Why this topic matters (Context)
Containers are the foundation of Kubernetes and platform engineering.  
To truly understand **Pods, resource limits, OOMKilled, HPA, and isolation**, you must understand **Linux namespaces and cgroups**, because **containers are built on top of these two kernel features**.

---

# Linux Namespaces

## Definition
Linux namespaces are a kernel feature that provides **isolation of system resources** by giving a process its own restricted view of the system.

In simple terms, namespaces control **what a process can see**.

---

## Use
Namespaces are used to:
- Isolate applications from each other
- Prevent processes from seeing or interfering with system-wide resources
- Create the illusion that a process is running on its own machine

They are used by:
- Docker
- containerd
- CRI-O
- Kubernetes (indirectly via container runtime)

---

## How namespaces work
When a process is started inside a namespace:
- The Linux kernel gives it a **limited view** of certain resources
- Other processes outside that namespace are hidden
- The process believes it is running alone

Namespaces do **not** limit resources — they only isolate visibility.

---

## Important Types of Namespaces

### PID Namespace
**Definition:**  
Isolates process IDs.

**Use:**  
Ensures a container cannot see host or other container processes.

**How it works:**  
A process inside the container sees itself as PID 1, even though it has a different PID on the host.

---

### Network Namespace
**Definition:**  
Isolates networking components such as interfaces, IP addresses, routing tables, and ports.

**Use:**  
Allows containers to have their own IP addresses and listen on the same ports without conflict.

**How it works:**  
Each namespace gets its own virtual network stack.  
In Kubernetes, **each Pod gets one network namespace**, shared by all containers in the Pod.

---

### Mount (MNT) Namespace
**Definition:**  
Isolates filesystem mount points.

**Use:**  
Ensures containers see only their own filesystem and mounted volumes.

**How it works:**  
The kernel provides a separate mount table for the process.

---

### UTS Namespace
**Definition:**  
Isolates hostname and domain name.

**Use:**  
Allows containers to have their own hostname.

**How it works:**  
Changes to hostname inside the container do not affect the host.

---

### IPC Namespace
**Definition:**  
Isolates inter-process communication resources.

**Use:**  
Prevents processes from sharing memory or message queues unintentionally.

**How it works:**  
Processes inside different IPC namespaces cannot communicate using shared memory mechanisms.

---

### User Namespace
**Definition:**  
Isolates user and group IDs.

**Use:**  
Improves security by mapping container root to a non-root user on the host.

**How it works:**  
A process can be root inside the container but unprivileged outside.

---

## Namespace Summary
> **Namespaces control WHAT a process can see.**  
They provide isolation, not resource control.

---

# cgroups (Control Groups)

## Definition
cgroups (control groups) are a Linux kernel feature that **limits, accounts for, and isolates resource usage** of processes.

In simple terms, cgroups control **how much a process can use**.

---

## Use
cgroups are used to:
- Prevent one workload from consuming all resources
- Enforce CPU and memory limits
- Provide fair resource sharing
- Protect system stability

They are used by:
- Docker
- Kubernetes
- systemd
- Cloud platforms

---

## How cgroups work
1. A cgroup is created
2. Processes are attached to the cgroup
3. Resource limits are defined
4. The Linux kernel enforces those limits

Enforcement happens **inside the kernel**, not in Kubernetes.

---

## Important cgroup Controllers

### CPU cgroup
**Definition:**  
Controls CPU usage.

**Use:**  
Prevents CPU starvation and ensures fair scheduling.

**How it works:**  
If a container exceeds its CPU limit, the kernel **throttles** it instead of killing it.

---

### Memory cgroup
**Definition:**  
Controls memory usage.

**Use:**  
Prevents a process from exhausting system memory.

**How it works:**  
If memory usage exceeds the limit, the kernel **kills the process** (OOMKilled, exit code 137).

---

### PIDs cgroup
**Definition:**  
Limits the number of processes.

**Use:**  
Prevents fork bombs and runaway process creation.

**How it works:**  
Kernel blocks creation of new processes beyond the limit.

---

## cgroups Summary
> **cgroups control HOW MUCH a process can use.**  
They enforce limits and protect the system.

---

# Namespaces vs cgroups (Very Important)

| Aspect | Namespaces | cgroups |
|-----|-----------|--------|
| Purpose | Isolation | Resource control |
| Controls | Visibility | Usage |
| Question answered | What can I see? | How much can I use? |
| Example | Own network stack | 256Mi memory limit |
| Failure result | Isolation boundary | Throttling / OOMKilled |

---

# How Kubernetes Uses Namespaces and cgroups

## Kubernetes Role
Kubernetes:
- **Does not create namespaces directly**
- **Does not enforce limits itself**

Instead:
- Kubernetes sends configuration to the container runtime
- The runtime configures namespaces and cgroups
- The Linux kernel enforces them

---

## Mapping in Kubernetes
- **Pod** → Network namespace
- **Container** → Process inside namespaces
- **resources.limits** → cgroups
- **OOMKilled** → kernel action, not Kubernetes

---

# Key Interview Takeaways

- Containers are **normal Linux processes**
- Namespaces provide **isolation**
- cgroups provide **resource limits**
- Kubernetes relies on the **Linux kernel**
- CPU limits throttle, memory limits kill

---

## One-line Interview Answer
> “Containers are Linux processes isolated using namespaces and constrained using cgroups, with Kubernetes relying on the kernel to enforce both.”

---

## Status
- Topic understood conceptually ✔
- Lab validated behavior ✔
- Ready for HPA & autoscaling topics ✔

---
