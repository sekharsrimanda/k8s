# 1‑Page Learning Notes

Taints and Tolerations are Kubernetes mechanisms used to control which Pods can run on which Nodes.
They work in the opposite direction of labels/selectors.

## Why they exist

By default, the Kubernetes scheduler is inclusive:

“Any Pod can run on any Node if resources are available.”

This becomes a problem when you need:

Dedicated nodes (GPU, infra, database)

Isolation (prod vs non‑prod)

Protection of special nodes (masters, critical workloads)

Taints and tolerations introduce an exclusion-first model:

Taint (Node side): “Do NOT schedule Pods here”

Toleration (Pod side): “I am allowed to ignore this restriction”

Taints (Node)

A taint is applied to a Node and has three parts:

key=value:effect


Effects:

NoSchedule → Pod will not be scheduled unless it tolerates

PreferNoSchedule → Scheduler avoids but may place if needed

NoExecute → Existing Pods are evicted unless they tolerate

Taints repel Pods by default.

Tolerations (Pod)

A toleration is defined on a Pod spec and allows it to be scheduled onto a tainted node.

## Important:

Tolerations do not attract Pods

They only permit scheduling

Scheduler still checks resources, affinity, etc.

Taints vs Node Affinity

Taints/Tolerations: “Who is NOT allowed here”

Node Affinity: “I WANT to run here”

In production, they are often used together.

Real‑World Examples

Control-plane nodes are tainted to block workloads

GPU nodes are tainted so only ML Pods run there

Infra nodes (logging, monitoring) are protected from app Pods

Failure Modes (Interview‑Important)

Pod stuck in Pending → taint not tolerated

Node drain evicts Pods → NoExecute taint

Overusing taints → scheduling deadlocks

5 Key Commands
# List node taints
kubectl describe node <node-name>

# Add a taint
kubectl taint nodes node1 dedicated=infra:NoSchedule

# Remove a taint
kubectl taint nodes node1 dedicated=infra:NoSchedule-

# View Pods and nodes together
kubectl get pods -o wide

# Debug unschedulable Pods
kubectl describe pod <pod-name>

# 5 Interview Q&A

Q1. What problem do taints solve?
Taints prevent unwanted Pods from being scheduled on specific Nodes, enabling isolation and protection.

Q2. Do tolerations guarantee Pod placement?
No. They only allow scheduling; they do not force it.

Q3. Difference between NoSchedule and NoExecute?
NoSchedule blocks new Pods; NoExecute also evicts existing Pods.

Q4. How are taints different from node selectors?
Taints repel Pods by default; selectors/affinity attract Pods explicitly.

Q5. Why are control-plane nodes tainted?
To prevent application workloads from interfering with cluster-critical components.

# Learning Resources
🎥 Video (Direct)

Kubernetes Taints & Tolerations (clear + practical):
https://www.youtube.com/watch?v=E1kN5Ew7KzM

Scheduler deep dive (includes taints):
https://www.youtube.com/watch?v=9dJk1rXfZ5A

# 📘 Official Documentation

Taints and Tolerations — Kubernetes Official Docs
https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/

Assigning Pods to Nodes
https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/

## Summary (Mental Model)

Taints protect Nodes.
Tolerations grant permission.
Affinity expresses preference.
