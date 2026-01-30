## Day 11 Interview Notes

Q: What caused the Pod to fail?
A: Kernel OOM killer triggered due to memory cgroup limit.

Q: Did Kubernetes kill the container?
A: No. The Linux kernel enforced the cgroup limit.

Q: Where are namespaces defined in YAML?
A: They are not. Container runtime configures them automatically.

Q: CPU limit vs memory limit behavior?
A: CPU throttles, memory kills.

Q: What is a container fundamentally?
A: A Linux process with namespaces and cgroups.
