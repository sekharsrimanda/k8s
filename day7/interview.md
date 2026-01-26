# Day 7 - Probes Interview Q&A

## Q1) What is a Readiness Probe?
Controls whether the Pod is ready to receive traffic. If it fails, Pod stays NotReady.

## Q2) What is a Liveness Probe?
Checks if the container is alive. If it fails repeatedly, kubelet restarts the container.

## Q3) What is a Startup Probe?
Used for slow-start apps. It delays liveness/readiness until startup succeeds.

## Q4) What happens if Readiness fails?
Pod stays running but is removed from Service endpoints (no traffic).

## Q5) What happens if Liveness fails?
Container gets restarted (may lead to CrashLoopBackOff if continuous).

## Q6) How do you debug probe failures?
Use describe pod (events), logs, and check probe config (path/port/timeouts).
