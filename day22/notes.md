#  🧾 1‑Page Notes — Pod Security Admission (PSA)

## What PSA Is
Pod Security Admission is a built‑in Kubernetes admission controller that validates pods against predefined security standards during creation or update. It enforces safer defaults without writing complex custom policies.

## Why It Exists
PodSecurityPolicy was removed due to complexity. PSA simplifies cluster security by using labels applied at the namespace level to control pod privileges.

###  Security Levels

Privileged — allows full access, minimal restriction

Baseline — blocks obvious risky configurations (host namespaces, privilege escalation)

Restricted — hardened setup (non‑root containers, limited capabilities)

### Label Modes

enforce  → rejects non‑compliant pods
audit    → records violations in logs
warn     → shows warnings during kubectl apply


### How Platform Engineers Use PSA

Apply baseline in shared dev environments

Roll out restricted gradually to production

Start with audit/warn before enabling enforce

### Key Behavior

PSA evaluates pods only at admission time

Namespace labels define which policy applies

Version labels can lock standards to a Kubernetes version

## ⚙️ 5 Key Commands
kubectl get namespace --show-labels

kubectl label namespace dev pod-security.kubernetes.io/enforce=baseline

kubectl label namespace prod pod-security.kubernetes.io/enforce=restricted --overwrite

kubectl label namespace prod pod-security.kubernetes.io/audit=restricted

kubectl describe namespace prod

## 🎯 Interview Q&A

### 1) What does PSA do in Kubernetes?
It validates pods against built‑in security standards during admission.

### 2) How is PSA configured?
Through namespace labels rather than custom policy objects.

### 3) Why use audit or warn modes first?
To observe violations safely before enforcing blocking rules.

### 4) What security level is usually recommended for production?
Restricted, after testing compatibility.

5) Does PSA continuously scan running pods?
No, it checks only when pods are created or updated.
