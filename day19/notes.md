# 1️⃣ What is a NetworkPolicy? (Definition)

A NetworkPolicy is a Kubernetes resource that controls network traffic to and from Pods using label-based rules.

It answers:

“Which Pods are allowed to talk to which other Pods (and on which ports)?”

By default, Kubernetes networking is allow-all.
NetworkPolicies introduce explicit allow rules.

## 2️⃣ How NetworkPolicy Works (Mental Model)

NetworkPolicy applies to Pods selected by labels

Rules define Ingress (incoming) and/or Egress (outgoing) traffic

Once a Pod is selected by any NetworkPolicy:

Traffic is DENIED by default

Only explicitly allowed traffic works

⚠️ NetworkPolicy is enforced by the CNI plugin, not the scheduler or kube-proxy.

## 3️⃣ Prerequisite (Conceptual — Important)

NetworkPolicies work only if your CNI supports them (e.g., Calico, Cilium, Weave).
Most learning clusters (including Killercoda) do support NetworkPolicy, but enforcement depends on the CNI.

## 4️⃣ Types of Traffic Control
🔹 Ingress

Controls who can reach the Pod.

🔹 Egress

Controls where the Pod can send traffic.

You can define:

Ingress only

Egress only

Both

## 5️⃣ Selectors You Must Know

podSelector → select target Pods

namespaceSelector → allow traffic from namespaces

ipBlock → allow CIDR ranges

These can be combined for precise control.

## 6️⃣ Default Deny (Core Pattern)

Creating a NetworkPolicy with:

podSelector: {}

No allowed rules

Results in:

All traffic blocked for that namespace’s Pods

This is the foundation of zero-trust networking in Kubernetes.

## 7️⃣ Common Real-World Scenarios

Frontend can talk to backend, but not database

Only monitoring namespace can scrape metrics

CI jobs blocked from production services

Apps break after adding NetworkPolicy (very common)

## 8️⃣ What NetworkPolicy Does NOT Do (Interview Traps)

❌ Does not control Service type
❌ Does not manage TLS or encryption
❌ Does not work without CNI support
❌ Does not apply to traffic from the node itself (unless CNI supports it)

## 9️⃣ Debugging NetworkPolicy Issues (High-Value Skill)

When traffic breaks:

Confirm Pods are selected by a policy

Check ingress vs egress rules

Verify namespace labels

Ensure ports & protocols match

Confirm CNI supports enforcement

## 5️⃣ Key Commands
kubectl get networkpolicy
kubectl describe networkpolicy <name>
kubectl get pods --show-labels
kubectl get ns --show-labels
kubectl describe pod <pod>

## 5️⃣ Interview Q&A

### Q1. What happens when a Pod is selected by a NetworkPolicy?
All traffic is denied except what is explicitly allowed.

### Q2. Is NetworkPolicy allow-all or deny-all by default?
Kubernetes is allow-all; NetworkPolicy introduces deny-by-default.

### Q3. Who enforces NetworkPolicy?
The CNI plugin (not Kubernetes core).

### Q4. Ingress vs Egress policies?
Ingress controls incoming traffic; Egress controls outgoing traffic.

### Q5. Why do apps suddenly stop communicating after adding NetworkPolicy?
Required traffic was not explicitly allowed.

## 📚 Official Documentation

https://kubernetes.io/docs/concepts/services-networking/network-policies/

## 🎥 Direct Video Links

TechWorld with Nana — Network Policies Explained
https://www.youtube.com/watch?v=0Omvgd7Hg1I

KodeKloud — Kubernetes Network Policies
https://www.youtube.com/watch?v=QYzZ3M5e8N8
