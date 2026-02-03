1‑Page Learning Notes
##Goal of PodDisruptionBudgets

Ensure application availability during voluntary disruptions such as:

Node drains

Cluster upgrades

Autoscaler scale‑downs

Manual pod evictions

PDBs protect workloads from being over‑disrupted at the same time.

Definition

A PodDisruptionBudget (PDB) is a Kubernetes policy object that limits how many Pods of a workload can be voluntarily evicted simultaneously.

PDBs do not prevent failures — they only control voluntary disruptions.

What PDB Controls (Very Important)

✅ Voluntary disruptions (drain, eviction API)

❌ Involuntary disruptions (node crash, OOMKill, kernel panic)

How PDB Works (Conceptual Flow)

You define a PDB using a label selector

Kubernetes calculates how many Pods are allowed to be unavailable

When an eviction is requested:

If budget allows → eviction succeeds

If budget violated → eviction is blocked

PDB Configuration Models
1️⃣ minAvailable

Guarantees a minimum number of Pods must stay running

Example:

minAvailable: 2

2️⃣ maxUnavailable

Limits maximum Pods that can be down

Example:

maxUnavailable: 1


⚠️ You must use only one of the two.

Why Platform Engineers Care

Safe node upgrades

Predictable autoscaling

Protects stateful and critical services

Prevents cascading outages

Critical Platform Insight (Interview Gold)

If every workload has replicas: 1 and a strict PDB → cluster upgrades will fail.

PDBs must align with:

Replica count

SLOs

Maintenance strategy

Common Misconceptions

❌ “PDB prevents pod crashes” → No

❌ “PDB guarantees uptime” → No

✅ “PDB limits simultaneous evictions” → Yes

5 Key Commands
kubectl get pdb
kubectl describe pdb <pdb-name>
kubectl get pods -l app=<label>
kubectl drain <node-name> --ignore-daemonsets
kubectl get events

5 Interview Q&A

Q1: What problem does a PodDisruptionBudget solve?
A: It prevents too many Pods from being evicted at the same time during voluntary disruptions.

Q2: Do PDBs protect against node crashes?
A: No. They only apply to voluntary disruptions.

Q3: What happens if a PDB is violated during drain?
A: The eviction request is rejected and the drain operation blocks.

Q4: minAvailable vs maxUnavailable — when to use which?
A:

minAvailable for strict availability guarantees

maxUnavailable for flexible scaling environments

Q5: Why can PDBs break cluster upgrades?
A: If replicas are too low, Kubernetes cannot evict Pods while respecting the budget.

Learning Resources (MANDATORY)
📘 Official Documentation

https://kubernetes.io/docs/tasks/run-application/configure-pdb/

https://kubernetes.io/docs/concepts/workloads/pods/disruptions/

🎥 Direct Video Links

Kubernetes PodDisruptionBudget explained (TechWorld with Nana):
https://www.youtube.com/watch?v=H5V9R3o5hJ0

PDB Deep Dive (Kubernetes SIG Apps):
https://www.youtube.com/watch?v=3Hk6O0JZ9gM
