# Day 24 — Admission Controllers (Validating & Mutating)

## Goal
Block privileged containers using ValidatingAdmissionPolicy.

## Steps
kubectl apply -f namespace.yaml
kubectl apply -f validating-policy.yaml
kubectl apply -f validating-binding.yaml

## Break
kubectl apply -f bad-pod.yaml

## Fix
kubectl apply -f good-pod.yaml
