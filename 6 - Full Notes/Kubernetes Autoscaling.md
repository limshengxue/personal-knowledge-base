2026-10-03 18:49

Tags: [[3 - Tags/kubernetes|kubernetes]]

# Kubernetes Autoscaling
- Autoscaling changes workload or cluster capacity according to observed demand and configured rules.
- Different controllers change different resources; increasing replicas is not the same as adding nodes.

## Three Boundaries
- Horizontal Pod Autoscaler: changes replica counts of supported workloads such as [[Kubernetes Deployment]].
- Vertical Pod Autoscaler: adjusts resource recommendations/requests through its installed components and policy.
- Node/cluster autoscaling: changes available node capacity through a compatible infrastructure integration.

## Inputs and Constraints
- Metrics must be available through the required APIs; Metrics Server commonly supplies resource metrics.
- Bounds, stabilization, resource requests, quotas, and underlying provisioning affect outcomes.
- A workload can request more replicas while pods remain pending because no node can host them.
- Vertical changes can affect workload lifecycle; inspect the installed implementation's update behavior.
- Avoid conflicting controllers independently managing the same resource dimension.

Use [[Kubernetes Resource Quotas and Limits]] to understand namespace constraints and [[Kubernetes Observability]] to inspect the signals driving changes.

# References
[[17 - Advanced Concepts]]
[Autoscaling workloads](https://kubernetes.io/docs/concepts/workloads/autoscaling/)
