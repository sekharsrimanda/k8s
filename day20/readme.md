Day 22 — RBAC Hands-on

Resources:
- Namespace dev
- ServiceAccount dev-sa
- Role pod-reader
- RoleBinding read-pods-binding

Break:
ServiceAccount lacked create verb.

Fix:
Added create to Role rules.
