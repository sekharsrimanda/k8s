# Day 21 — Network Policies (Killercoda)

## Prerequisites
- CNI with NetworkPolicy support (e.g., Calico)
- Two namespaces: frontend, backend

## What I Built
- Default-deny ingress policy for backend
- Allow rule permitting frontend → backend on TCP 80

## Break & Fix
- Break: Default-deny blocked all ingress
- Fix: Added explicit allow rule using namespace + pod selectors

## Validation
- curl from frontend to backend-svc
