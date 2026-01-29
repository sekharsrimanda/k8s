# Platform Engineering — Kubernetes Interview-Prep Textbook
## Coverage: Day 1 to Day 10 (Complete & Exhaustive)

This document is written as a **textbook**, not notes.
Each chapter explains:
- What the concept is
- Why it exists
- How Kubernetes implements it internally
- How to explain it in interviews

------------------------------------------------------------

# Chapter 1 — Kubernetes Architecture

## Kubernetes (Overall System)

Definition:
Kubernetes is a distributed container orchestration system designed to manage the lifecycle of containerized applications by maintaining a desired state across a cluster of machines.

Use:
Kubernetes is used to run applications reliably at scale by automating deployment, scaling, self-healing, configuration, and networking. It abstracts infrastructure complexity so teams focus on applications instead of servers.

How it works:
Kubernetes follows a declarative model. Users define the desired state (for example, number of replicas, resources, networking). Kubernetes continuously monitors the actual state and uses control loops to reconcile differences until the desired state is achieved.

---

## Control Plane

The control plane is responsible for **decision-making**.
Worker nodes are responsible for **execution**.

This separation is fundamental to Kubernetes scalability and reliability.

---

## API Server

Definition:
The API Server is the central management component and the only entry point to the Kubernetes cluster.

Use:
It is used to validate, authenticate, authorize, and acknowledge all requests that modify or read cluster state.

How it works:
Every request (kubectl, controllers, scheduler, external tools) goes through the API Server.
Steps:
1. Request is authenticated
2. Authorization is enforced
3. Admission controllers validate and mutate requests
4. State is persisted in etcd
5. Other components observe changes via watches

Interview insight:
If the API Server is down, workloads keep running, but **no changes** can be made to the cluster.

---

## etcd

Definition:
etcd is a distributed, strongly consistent key-value store used as Kubernetes’ backing store.

Use:
It stores the complete cluster state, including configuration, metadata, and desired state.

How it works:
Only the API Server communicates with etcd. Every cluster change is written to etcd, making it the single source of truth.

Interview insight:
If etcd data is lost, the cluster state is lost, even if Pods are still running.

---

## Scheduler

Definition:
The Scheduler assigns Pods to Nodes.

Use:
It decides where Pods should run based on resource availability and constraints.

How it works:
For unscheduled Pods:
1. Filters nodes based on constraints
2. Scores remaining nodes
3. Selects the best node
4. Binds the Pod to that node

Important:
Scheduler uses **resource requests**, not limits.

---

## Controller Manager

Definition:
The Controller Manager runs multiple controllers that ensure the cluster matches the desired state.

Use:
It enables self-healing and automation.

How it works:
Each controller runs a reconciliation loop:
- Observe state
- Compare with desired state
- Take corrective action via the API Server

---

## Worker Node Components

### kubelet
Definition:
An agent running on each worker node.

Use:
Ensures containers described in Pod specs are running.

How it works:
kubelet watches assigned Pods, interacts with the container runtime, executes probes, enforces limits, and reports status.

### kube-proxy
Definition:
Networking component that implements Service routing.

Use:
Routes traffic from Services to Pods.

How it works:
Programs iptables/IPVS rules; does not proxy traffic at application level.

------------------------------------------------------------

# Chapter 2 — Pods, Labels, and Selectors

## Pod

Definition:
A Pod is the smallest deployable unit in Kubernetes and encapsulates one or more containers.

Use:
Used when containers must share networking, storage, and lifecycle.

How it works:
All containers in a Pod share the same IP address, ports, and volumes. Pods are ephemeral and managed by higher-level controllers.

---

## Labels

Definition:
Labels are key-value metadata attached to Kubernetes objects.

Use:
Used for grouping, selection, organization, and management.

How it works:
Labels do not create relationships. They enable **dynamic discovery** using selectors.

---

## Selectors

Definition:
Selectors are queries that match objects based on labels.

Use:
Used by Services, ReplicaSets, Deployments, monitoring tools, and policies.

How it works:
Selectors dynamically resolve matching Pods at runtime. Changes in labels automatically affect selection.

------------------------------------------------------------

# Chapter 3 — ReplicaSets and Deployments

## ReplicaSet

Definition:
A ReplicaSet ensures a specified number of Pods are running at all times.

Use:
Provides self-healing and scaling.

How it works:
It continuously compares desired replicas with actual Pods and creates or deletes Pods accordingly.

---

## Deployment

Definition:
A Deployment manages ReplicaSets and provides declarative updates.

Use:
Used to deploy applications safely and manage rollouts.

How it works:
When updated, a new ReplicaSet is created. Old Pods are gradually replaced using rolling update strategy.

Interview insight:
You never update Pods directly; you update Deployments.

------------------------------------------------------------

# Chapter 4 — Services & Networking

## Service

Definition:
A Service provides a stable network endpoint for a dynamic set of Pods.

Use:
Used to abstract Pod IP changes and enable service discovery.

How it works:
Services select Pods using labels. kube-proxy routes traffic to backend Pods.

---

## ClusterIP

Definition:
Default Service type that exposes applications internally.

Use:
Used for internal microservice communication.

How it works:
Creates a virtual IP accessible only inside the cluster.

---

## Node IP

Definition:
IP address assigned to a Kubernetes node.

Use:
Used for node communication and NodePort access.

How it works:
Traffic sent to NodeIP may be forwarded internally by kube-proxy rules.

---

## NodePort

Definition:
Service type that exposes a Service on a static port on every node.

Use:
Used for simple external access and testing.

How it works:
Traffic to `<NodeIP>:NodePort` is forwarded to the Service, then to Pods.

---

## LoadBalancer

Definition:
Service type that exposes applications using a cloud provider load balancer.

Use:
Used for production external access.

How it works:
Cloud load balancer forwards traffic to NodePorts, which route to Pods.

---

## Service Traffic Flow

How it works:
Client → Service IP / Node IP / LB IP → kube-proxy → Pod IP

------------------------------------------------------------

# Chapter 5 — Ingress

## Ingress

Definition:
Ingress is an API object that manages HTTP/HTTPS routing to Services.

Use:
Used to expose multiple services through a single entry point.

How it works:
Ingress rules are implemented by an Ingress Controller that configures a reverse proxy.

---

## Host-based Routing
Routes traffic based on hostname.

## Path-based Routing
Routes traffic based on URL paths.

------------------------------------------------------------

# Chapter 6 — Probes

## Liveness Probe

Definition:
Checks if a container is alive.

Use:
Used to restart stuck containers.

How it works:
Failure triggers container restart by kubelet.

---

## Readiness Probe

Definition:
Checks if a Pod can receive traffic.

Use:
Used to control traffic flow.

How it works:
Failure removes Pod from Service endpoints without restarting it.

---

## Startup Probe

Definition:
Checks if an application has started.

Use:
Protects slow-starting applications.

How it works:
Disables liveness and readiness until startup completes.

---

## Probe Methods

### httpGet
Uses HTTP response codes.

### tcpSocket
Checks port connectivity.

### exec
Runs command inside container.

------------------------------------------------------------

# Chapter 7 — ConfigMaps and Secrets

## ConfigMap

Definition:
Stores non-sensitive configuration data.

Use:
Used to externalize configuration from code.

How it works:
Injected via environment variables or mounted files.

---

## Secret

Definition:
Stores sensitive data.

Use:
Used to manage credentials securely.

How it works:
Base64-encoded values injected similarly to ConfigMaps, with access controls.

------------------------------------------------------------

# Chapter 8 — Resource Requests and Limits

## Resource Requests

Definition:
Minimum guaranteed CPU and memory.

Use:
Used by scheduler for placement decisions.

How it works:
Scheduler ensures sufficient allocatable resources exist.

---

## Resource Limits

Definition:
Maximum allowed CPU and memory.

Use:
Used to prevent resource exhaustion.

How it works:
CPU is throttled; memory overuse causes OOMKilled.

------------------------------------------------------------

# Chapter 9 — OOMKilled

## OOMKilled

Definition:
Indicates a container was terminated due to memory limit violation.

Use:
Signals misconfigured memory limits.

How it works:
Linux kernel kills the container process with exit code 137.

------------------------------------------------------------

# FINAL STATUS

Chapters covered: 1–9  
Days completed: 10  
Foundation strength: Interview-ready  
Next topics: HPA, Helm, Terraform, Platform Automation

------------------------------------------------------------
