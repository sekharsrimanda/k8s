# 1‑Page Learning Notes
## What problem Jobs & CronJobs solve

Not all workloads are long‑running services. Many platform tasks are finite or scheduled:

Database migrations

Backups

Batch processing

Periodic cleanup

Jobs and CronJobs are Kubernetes primitives for run‑to‑completion workloads.

## Job (Run Once / Finite)

A Job creates one or more Pods and retries them until completion.

## Key properties:

Completion‑based, not replica‑based

Tracks successful completions

Can retry on failure

## Important fields:

completions – total successful Pods required

parallelism – how many Pods run at the same time

backoffLimit – retries before marking failed

## CronJob (Scheduled Jobs)

A CronJob creates Jobs on a schedule, similar to Linux cron.

## Key properties:

Uses cron syntax

Suitable for periodic tasks

Creates Jobs, which then create Pods

## Important fields:

schedule – cron expression

concurrencyPolicy

Allow (default)

Forbid

Replace

startingDeadlineSeconds

## Job vs CronJob
Aspect	Job	CronJob
Trigger	Manual / event	Time‑based
Lifetime	Finite	Repeating
Creates	Pods	Jobs → Pods
Production Gotchas (Interview‑Critical)

Forgetting cleanup → too many completed Jobs

Wrong cron timezone assumptions

High retry limits causing resource pressure

Parallel Jobs overwhelming downstream systems

## 5 Key Commands
kubectl get jobs
kubectl describe job <job-name>

kubectl get cronjobs
kubectl describe cronjob <cronjob-name>

kubectl get pods --selector=job-name=<job-name>

## 5 Interview Q&A

## Q1. When do you use a Job instead of a Deployment?
A Job is for finite, run‑to‑completion tasks; Deployments are for long‑running services.

## Q2. How does Kubernetes know a Job is complete?
When the required number of successful Pod completions is reached.

## Q3. What happens if a Job Pod fails?
Kubernetes retries it based on backoffLimit.

## Q4. How does a CronJob work internally?
It creates a Job on schedule; the Job then creates Pods.

## Q5. How do you prevent overlapping CronJobs?
Use concurrencyPolicy: Forbid or Replace.
