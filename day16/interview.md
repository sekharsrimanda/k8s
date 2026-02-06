# Day 17 — Interview Notes: Affinity

## Q1. Why did the Pod stay Pending?
Required node affinity could not be satisfied by any node.

## Q2. Difference between required and preferred affinity?
Required blocks scheduling; preferred is best-effort.

## Q3. Why use pod anti-affinity?
To spread replicas for high availability.

## Q4. What does topologyKey control?
The boundary (node/zone) for affinity rules.

## Q5. Affinity vs Taints?
Affinity guides Pod placement; taints protect nodes.
