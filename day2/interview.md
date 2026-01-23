✅ DAY-2 — ConfigMap + Secret + Environment Variables + Break/Fix
📌 Definitions (Day-2 Concepts)
1) ConfigMap

Definition: A ConfigMap stores non-sensitive configuration data like environment variables, application settings, or config files.

2) Secret

Definition: A Secret stores sensitive data like passwords, tokens, and keys used by applications.

3) env in Kubernetes

Definition: env is used to set environment variables inside a container.

4) envFrom

Definition: envFrom loads all key-value pairs from a ConfigMap or Secret as environment variables inside a container.

5) configMapKeyRef

Definition: It injects a specific key from a ConfigMap into an environment variable.

6) secretKeyRef

Definition: It injects a specific key from a Secret into an environment variable.

7) CreateContainerConfigError

Definition: A Pod error status that occurs when container configuration fails (missing ConfigMap/Secret keys, invalid env references, etc.).

8) Base64 encoding in Secrets

Definition: Secret values are stored and shown as base64 encoded strings (not plain text) in YAML output.

🎯 Day-2 Interview Questions & Answers
Q1) What is the difference between ConfigMap and Secret?

✅ Answer:
ConfigMap is for non-sensitive config. Secret is for sensitive data like passwords and tokens.

Q2) How do you use ConfigMap/Secret inside a Pod?

✅ Answer:
You can inject them as:

Environment variables (env, envFrom)

Mounted files (volumes)

Q3) Are Kubernetes Secrets encrypted?

✅ Answer:
By default they are base64 encoded, not encrypted. Encryption at rest must be enabled in the cluster.

Q4) What happens if a ConfigMap is deleted?

✅ Answer:
Existing pods may continue running, but new pods can fail if they depend on that ConfigMap (especially during restart).

Q5) What is CreateContainerConfigError?

✅ Answer:
It means the container config is invalid, often due to missing ConfigMap/Secret keys or incorrect references.

Q6) How do you verify env variables inside a running pod?

✅ Answer:
Use:

kubectl exec -it <pod> -- env

Q7) Why should we avoid hardcoding passwords in YAML?

✅ Answer:
It’s insecure and not manageable. Use Secrets + RBAC instead.
