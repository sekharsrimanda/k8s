# 🎯 1‑Page Notes

RBAC controls who can do WHAT on WHICH Kubernetes resources.
It is the main authorization system used by Kubernetes to enforce least‑privilege access across users, service accounts, and automation tools.

RBAC works using the API group rbac.authorization.k8s.io and evaluates permissions whenever a request hits the Kubernetes API.

## 🧩 Core RBAC Objects

### Role

Namespace‑scoped permissions

Example: allow reading pods in frontend

### ClusterRole

Cluster‑wide permissions

Used for nodes, CRDs, or global read access

### RoleBinding

Attaches a Role to a user/service account inside a namespace

### ClusterRoleBinding

Attaches a ClusterRole cluster‑wide

These four objects define the entire RBAC model.

## 🔐 How RBAC Decisions Work (Important Mental Model)

Every API request asks:

Can SUBJECT perform VERB on RESOURCE in NAMESPACE?


Example:

Can dev-user create deployments in backend?


## RBAC evaluates:

apiGroups

resources

verbs (get, list, create, update, delete)

If no rule allows it → request denied.

### 📊 Role vs ClusterRole (Platform Engineer Perspective)
Resource	Scope	Real Usage
Role	Single namespace	Team isolation
ClusterRole	Entire cluster	Platform‑level permissions
## 🧠 Key Platform Engineering Concepts

RBAC enforces least privilege

Prevents accidental cluster damage

Used heavily with GitOps & CI/CD

Works together with NetworkPolicies & Namespaces

🎬 Learning Videos
📚 Official Documentation

Using RBAC Authorization (Official Kubernetes Docs)

Kubernetes Authorization Overview

🧾 5 Key Commands
kubectl api-resources
kubectl get roles -A
kubectl get rolebindings -A
kubectl auth can-i create pods --as=user1
kubectl describe clusterrole cluster-admin

🎤 5 Interview Q&A

Q1. What is RBAC in Kubernetes?
A: A role‑based authorization system controlling access to API resources using Roles and Bindings.

Q2. Difference between Role and ClusterRole?
A: Role = namespace‑scoped permissions; ClusterRole = cluster‑wide permissions.

Q3. Does RBAC deny rules exist?
A: No explicit deny rules; only allow rules are defined.

Q4. What happens if no Role matches a request?
A: Access is denied by default.

Q5. Why is RBAC critical for Platform Engineers?
A: It enforces least privilege, secures multi‑team clusters, and prevents unauthorized API actions.
