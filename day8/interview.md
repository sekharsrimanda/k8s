# Day 8 — ConfigMaps & Secrets (Interview Q&A)

## Q1: What is a ConfigMap?
A ConfigMap stores non-sensitive configuration data separately from application code.

## Q2: What is a Secret?
A Secret stores sensitive data like passwords, tokens, and keys (base64 encoded by default).

## Q3: How can Pods consume ConfigMaps and Secrets?
- As environment variables
- As mounted files via volumes

## Q4: Do ConfigMap or Secret updates automatically update running Pods?
No. Pods must be restarted to pick up changes when consumed as environment variables.

## Q5: How do you debug a Pod failing due to ConfigMap or Secret issues?
Using `kubectl describe pod`, checking events, and inspecting container logs.

## Q6: Why shouldn’t Secrets be stored in Git in plain text?
Because they contain sensitive information and should be protected via encryption and access controls.
