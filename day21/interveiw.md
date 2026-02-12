Q: What are Pod Security Standards?
A: Policies enforcing secure pod configurations (Privileged, Baseline, Restricted).

Q: What replaced PodSecurityPolicy?
A: Pod Security Admission.

Q: What does runAsNonRoot do?
A: Prevents containers from running as root user.

Q: Why drop ALL capabilities?
A: Reduce attack surface and privilege escalation.

Q: What is seccompProfile RuntimeDefault?
A: Default syscall filtering for container hardening.
