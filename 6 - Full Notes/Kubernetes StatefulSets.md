2026-10-03 18:49

Tags: [[3 - Tags/kubernetes|kubernetes]]

# Kubernetes StatefulSets
- StatefulSets maintain stable identities for pods whose identity matters across rescheduling.
- They support ordered management and association with persistent storage.
- They do not implement application-level database replication or backup.

## Identity and Storage
- Pods receive stable ordinal names.
- A governing headless Service provides the intended network identity.
- Volume claim templates can associate storage with each pod; see [[Kubernetes Volume]].
- A replacement pod can preserve its identity while running on a different node.

## Choosing a Workload
- Use [[Kubernetes Deployment]] when interchangeable replicas are appropriate.
- Consider StatefulSet when stable naming, ordered lifecycle, or per-pod storage is required.
- Database consistency, failover, and recovery still require application configuration or an operator.
- Inspect PVC retention policy before scaling down or deleting a StatefulSet; do not assume data is automatically removed.

A MySQL cluster is an example use case, not evidence that any MySQL image becomes a resilient cluster by choosing StatefulSet.

# References
[[17 - Advanced Concepts]]
[StatefulSets](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)
