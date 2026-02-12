Day 23 — Pod Security Standards & SecurityContext

Namespace:
- secure-ns with restricted Pod Security level

Break:
- Pod rejected due to missing securityContext

Fix:
- Added runAsNonRoot
- Dropped Linux capabilities
- Disabled privilege escalation
- Added RuntimeDefault seccomp profile
