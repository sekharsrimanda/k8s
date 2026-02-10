# Day 21 — NetworkPolicy Interview Notes

Q: What enforces NetworkPolicy?
A: The CNI plugin (e.g., Calico), not Kubernetes core.

Q: What happens when a Pod is selected by a NetworkPolicy?
A: Traffic is denied by default unless explicitly allowed.

Q: Ingress vs Egress?
A: Ingress controls incoming; Egress controls outgoing traffic.

Q: Why do apps break after adding NetworkPolicy?
A: Required traffic wasn’t explicitly allowed.

Q: Does NetworkPolicy provide encryption?
A: No. It controls traffic flow only.
