# Day 11 — Linux Namespaces & cgroups

This lab demonstrates:
- How Kubernetes enforces cgroups using resource limits
- How memory overuse triggers OOMKilled
- That containers are normal Linux processes constrained by the kernel

Files:
- pod-ok.yaml: Valid resource limits
- pod-broken.yaml: Intentional OOM via memory cgroup
- pod-fixed.yaml: Corrected limits

Key takeaway:
Namespaces isolate visibility.
cgroups enforce resource usage.
