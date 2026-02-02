# Day 14 — Interview Notes (VPA)

## Q1: What does VPA scale?
CPU and memory requests (and optionally limits).

## Q2: Does VPA scale replicas?
No.

## Q3: What happens in Auto mode?
Pods are evicted and recreated with new requests.

## Q4: Why is Off mode safer?
No Pod restarts; recommendation-only.

## Q5: Can VPA and HPA conflict?
Yes, if both act on the same resource.
