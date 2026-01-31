# Day 12 — HPA Interview Q&A

Q1) What is HPA?
Answer: Horizontal Pod Autoscaler automatically scales pod replicas based on resource utilization like CPU or memory.

Q2) What does HPA depend on?
Answer: Metrics Server and resource requests defined on containers.

Q3) Why CPU requests are mandatory for HPA?
Answer: HPA calculates CPU utilization as a percentage of requested CPU.

Q4) What happens if metrics-server is not installed?
Answer: HPA silently fails and pod count never changes.

Q5) How often does HPA check metrics?
Answer: Every ~15 seconds, with stabilization windows applied.

Q6) What does “metrics API not available” mean?
Answer: Metrics server is missing or unable to communicate with kubelets.

Q7) Why is --kubelet-insecure-tls required in Killer Coda?
Answer: Killer Coda uses self-signed certificates which metrics-server cannot verify by default.

Q8) Difference between HPA v1 and v2?
Answer: v2 supports multiple metrics and advanced scaling policies.

Q9) Can HPA scale down immediately?
Answer: No. Scale-down is intentionally slower to avoid flapping.

Q10) What are common HPA debugging steps?
Answer:
- Check metrics-server
- Verify CPU requests
- Check kubectl top
- Describe HPA for events
