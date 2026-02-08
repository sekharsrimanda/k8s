# 1-Page Learning Notes
## Definition

A DaemonSet is a Kubernetes controller that ensures exactly one Pod runs on every eligible node in the cluster.

## Why DaemonSets exist

Some workloads are node-level, not application-level.
They must run per node, regardless of replicas or traffic.

## Typical examples:

Log collectors (Fluent Bit, Filebeat)

Monitoring agents (Node Exporter)

Network plugins (CNI)

Storage plugins (CSI)

Security agents

Deployments cannot guarantee this behavior. DaemonSets can.

## How DaemonSets work (internals)

DaemonSet controller watches node list

## For every eligible node:

If Pod does not exist → create one

When a node is added → Pod is created

When a node is removed → Pod is deleted

No replicas field exists because scaling is based on node count.

## Scheduling rules

DaemonSet Pods still respect:

nodeSelector

node affinity

taints & tolerations

resource requests/limits

They are not immune to scheduling constraints.

## Update strategy

RollingUpdate (default)

OnDelete

Used carefully in production to avoid breaking infra agents.

## 5 Key Commands
kubectl get daemonsets
kubectl describe daemonset <name>
kubectl get pods -o wide
kubectl rollout status daemonset <name>
kubectl rollout history daemonset <name>

## 5 Interview Q&A

## Q1. What problem does a DaemonSet solve?
Ensures one Pod runs per node for node-level workloads.

## Q2. How does a DaemonSet scale?
Automatically with node count.

## Q3. Does a DaemonSet use replicas?
No. Replicas are implicit.

## Q4. Can DaemonSets run on control-plane nodes?
Yes, with tolerations.

## Q5. DaemonSet vs Deployment?
DaemonSet = node-based. Deployment = replica-based.

Learning Resources

Docs: https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/
