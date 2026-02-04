# Day 16 — Interview Notes: Taints & Tolerations

## Q1. Why did the Pod stay Pending?
Because the node had a NoSchedule taint and the Pod had no toleration.

## Q2. Do tolerations guarantee Pod placement?
No. They only allow scheduling; the scheduler still checks resources.

## Q3. What happens with NoExecute taint?
Existing Pods without toleration are evicted.

## Q4. Why are control-plane nodes tainted?
To prevent application workloads from affecting cluster stability.

## Q5. Taints vs Node Affinity?
Taints block by default; affinity attracts explicitly.

