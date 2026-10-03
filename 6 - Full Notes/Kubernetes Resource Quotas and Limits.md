2026-10-03 18:49

Tags: [[3 - Tags/kubernetes|kubernetes]]

# Kubernetes Resource Quotas and Limits
- Resource limits make shared-cluster consumption explicit at two different boundaries.
- `ResourceQuota` constrains aggregate usage in a [[Kubernetes Namespaces|namespace]].
- `LimitRange` constrains individual resources and can supply defaults.

## ResourceQuota
- Can constrain compute requests/limits, storage requests, and selected object counts.
- Admission rejects creation or updates that would exceed applicable quotas.
- A quota is not a reservation guaranteeing free node capacity.

## LimitRange
- Can impose minimum/maximum CPU and memory for pods or containers.
- Can constrain PVC storage requests and limit-to-request ratios.
- Defaults affect newly admitted workloads; they are not an automatic rewrite of every existing pod.

## Operational Checks
- Inspect `kubectl describe quota -n <namespace>` and `kubectl describe limitrange -n <namespace>`.
- Diagnose quota admission failures separately from scheduler capacity failures.
- [[Kubernetes Autoscaling]] changes desired capacity but does not bypass quotas.

# References
[[17 - Advanced Concepts]]
[Resource quotas](https://kubernetes.io/docs/concepts/policy/resource-quotas/)
[LimitRange](https://kubernetes.io/docs/concepts/policy/limit-range/)
