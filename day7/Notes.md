1️⃣ One‑Page Notes: Kubernetes Probes
What are Probes?

In Kubernetes, probes are health checks used by the kubelet to understand:

Is the container alive?

Is it ready to receive traffic?

Has it finished starting up?

They directly affect traffic routing, restarts, and application stability.

🔹 Readiness Probe

Definition:
Checks whether a container is ready to accept traffic.

Use‑case:

Control when traffic should be sent to a Pod.

Pod is removed from Service endpoints when probe fails.

Key Point:
❌ Does NOT restart the container
✅ Only affects traffic flow

Real systems:

App depends on DB / cache

During deployments or warm‑ups

🔹 Liveness Probe

Definition:
Checks whether a container is still alive.

Use‑case:

Detects stuck or deadlocked applications.

Key Point:
❌ If it fails → container is restarted
⚠️ Dangerous if misconfigured

Real systems:

Apps that may hang but not crash

Legacy apps without crash signals

🔹 Startup Probe

Definition:
Checks whether the application has started successfully.

Use‑case:

For slow‑starting applications.

Key Point:
⏳ Disables liveness & readiness until startup probe succeeds
✅ Prevents early restarts

Real systems:

Java / Spring Boot apps

Apps loading large configs or migrations

2️⃣ When to Use Which Probe?
Scenario	Probe
Control traffic flow	Readiness
Restart stuck containers	Liveness
Slow app startup	Startup
External dependency checks	Readiness
Avoid crash loops at boot	Startup

3️⃣ Probe Types
🔸 HTTP Probe
httpGet:
  path: /health
  port: 8080


Most common

Best for web apps

🔸 TCP Probe
tcpSocket:
  port: 3306


Checks if port is open

No app‑level logic

🔸 Exec Probe
exec:
  command: ["cat", "/tmp/healthy"]


Runs command inside container

Useful for legacy apps

4️⃣ Debugging Probe Failures (Must Know)
Step‑by‑step:

Describe Pod

kubectl describe pod <pod-name>


👉 Look for Events like:

Readiness probe failed

Liveness probe failed

Check Pod Status

kubectl get pods


👉 Watch for:

CrashLoopBackOff

NotReady

Check Logs

kubectl logs <pod-name>
kubectl logs <pod-name> -c <container>


Check Endpoint

kubectl get endpoints <service-name>


👉 Pod missing = readiness failed

Manual Curl

kubectl exec -it <pod> -- curl localhost:8080/health

4️⃣ Debugging Probe Failures (Must Know)
Step‑by‑step:

Describe Pod

kubectl describe pod <pod-name>


👉 Look for Events like:

Readiness probe failed

Liveness probe failed

Check Pod Status

kubectl get pods


👉 Watch for:

CrashLoopBackOff

NotReady

Check Logs

kubectl logs <pod-name>
kubectl logs <pod-name> -c <container>


Check Endpoint

kubectl get endpoints <service-name>


👉 Pod missing = readiness failed

Manual Curl

kubectl exec -it <pod> -- curl localhost:8080/health
