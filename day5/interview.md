📌 Definitions (Day-5 Concepts)
Secret

Definition: Secret is used to store sensitive data like passwords, tokens, API keys and inject into Pods securely.

envFrom + secretRef

Definition: envFrom loads all key-value pairs from Secret as environment variables inside container.

Secret Volume Mount

Definition: Mounting a Secret as volume creates files inside container, each key becomes a file.

Base64 Encoding

Definition: Kubernetes stores Secret data as base64 encoded values in YAML output.

🎯 Day-5 Interview Questions & Answers
Q1) What is a Secret in Kubernetes?

✅ Answer: Secret stores sensitive information like passwords/tokens and can be injected into Pods.

Q2) ConfigMap vs Secret difference?

✅ Answer:

ConfigMap = non-sensitive config

Secret = sensitive config

Q3) How can we use Secrets inside Pods?

✅ Answer: Using ENV variables (envFrom) or mounting as volume files.

Q4) What happens if Secret name is wrong in Pod YAML?

✅ Answer: Pod will fail to start and you can see the error in describe/events.

Q5) How do you troubleshoot Secret issues?

✅ Answer:

kubectl describe pod

kubectl get events

kubectl describe secret
