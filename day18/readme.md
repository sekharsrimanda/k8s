# Day XX — Jobs & CronJobs (Hands-on)

## Goal
Understand execution-based workloads using Job and CronJob.

## What I Built
- Job: one-time-job
- CronJob: scheduled-job

## Key Learnings
- Jobs run to completion
- CronJobs create Jobs on a schedule
- Pods reach Completed state

## Commands Used
kubectl apply -f *.yaml
kubectl get jobs
kubectl get cronjobs
kubectl get pods
kubectl logs <pod>
