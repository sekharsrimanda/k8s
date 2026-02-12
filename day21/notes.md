# Day 23 — Pod Security Standards & SecurityContext
## 🎯 1‑Page Notes

Pod Security Standards (PSS) define how secure a Pod must be allowed to run inside a Kubernetes cluster. They protect nodes from dangerous container configurations like privileged access, host networking, or unsafe capabilities.

Instead of controlling who can access the API (RBAC), this topic controls how Pods are allowed to run — a core Platform Engineering responsibility.

## 🔐 The 3 Pod Security Levels

### Privileged

Almost no restrictions

Used for infrastructure workloads

### Baseline

Prevents known privilege escalation

Safe default for most apps

### Restricted

Strong hardening rules (best practice security)

These policies are cumulative: Restricted ⟶ Baseline ⟶ Privileged.

## 🧩 What is securityContext?

A section in Pod/Container spec that defines runtime security settings:

Examples:

Run as non‑root user

Drop Linux capabilities

Set seccomp profile

Control filesystem permissions

Platform engineers use these fields to enforce secure defaults.

## 🛡️ Pod Security Admission (Important)

Kubernetes includes a built‑in admission controller that enforces Pod Security Standards at namespace level.

### Policies can run in modes:

Enforce → reject insecure Pods

Warn → allow but show warning

Audit → log violations

## 🧠 Platform Engineering Insight

### RBAC answers:

Who can deploy?


### Pod Security answers:

What kind of Pod is allowed?


Together they form the core Kubernetes security model.

### 🎬 Learning Videos
### 📚 Official Documentation

Pod Security Standards (Official Docs)

Pod Security Admission Controller Overview

Enforcing Pod Security Admission Policies

## 🧾 5 Key Commands
kubectl get ns --show-labels
kubectl label ns dev pod-security.kubernetes.io/enforce=baseline
kubectl describe ns dev
kubectl get pods -A -o yaml | grep securityContext
kubectl explain pod.spec.securityContext

## 🎤 5 Interview Q&A

### Q1. What problem do Pod Security Standards solve?
A: They restrict insecure Pod configurations and enforce workload hardening rules.

### Q2. Difference between RBAC and Pod Security?
A: RBAC controls API access; Pod Security controls runtime behavior of Pods.

### Q3. What replaced PodSecurityPolicy (PSP)?
A: Pod Security Admission + Pod Security Standards.

### Q4. What are the three security levels?
A: Privileged, Baseline, Restricted.

Q5. Why is securityContext important for Platform Engineers?
A: It enforces non‑root execution, capability drops, and hardened runtime settings.
