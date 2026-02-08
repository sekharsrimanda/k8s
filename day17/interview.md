
# Day 18 — DaemonSets (Interview Preparation)

## Q1. What is a DaemonSet?
A DaemonSet is a Kubernetes controller that ensures **one Pod runs on every eligible node** in the cluster.

---

## Q2. How does a DaemonSet scale?
DaemonSets scale **automatically with node count**.  
When a node is added, a Pod is created. When a node is removed, the Pod is deleted.

---

## Q3. Why doesn’t a DaemonSet have replicas?
Because replicas are implicit. The number of Pods equals the number of eligible nodes.

---

## Q4. Can DaemonSet Pods run on control-plane nodes?
Yes. DaemonSet Pods can run on tainted nodes if **appropriate tolerations** are defined.

---

## Q5. What happens if a DaemonSet has a nodeSelector that matches no nodes?
The DaemonSet is created, but **no Pods are scheduled**.  
This must be debugged using `kubectl describe daemonset`.

---

## Q6. DaemonSet vs Deployment?
- **DaemonSet:** Node-level, one Pod per node
- **Deployment:** Application-level, replica-based scaling

---

## Q7. How do you debug a DaemonSet with zero Pods?
- `kubectl describe daemonset <name>`
- `kubectl get events`
- Check node labels and taints

---

## Q8. Give real production use cases.
- Logging agents
- Monitoring agents
- Network plugins
- Storage plugins
- Security agents

---

## Key Interview Line
> “DaemonSets are used for node-level workloads and scale with nodes, not replicas.”
