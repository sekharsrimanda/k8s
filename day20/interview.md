Q: Role vs ClusterRole?
A: Role is namespace-scoped, ClusterRole is cluster-wide.

Q: What happens without RoleBinding?
A: Permissions are never applied.

Q: Does RBAC support deny rules?
A: No, only allow rules.

Q: Why use ServiceAccounts?
A: To grant workload-specific permissions.

Q: kubectl auth can-i usage?
A: Test authorization decisions.
