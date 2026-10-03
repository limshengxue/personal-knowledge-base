2026-10-03 18:49

Tags: [[3 - Tags/kubernetes|kubernetes]] [[Kubernetes Object Model]]

# Kubernetes DaemonSets
- A DaemonSet maintains a pod on each eligible node.
- It is useful for node-local agents such as logging collectors or infrastructure components.
- It does not express an arbitrary global replica count like [[Kubernetes Deployment]].

## Node Eligibility
- Node selection and affinity can restrict the target nodes.
- Taints and tolerations also affect placement.
- When an eligible node joins, the controller arranges the corresponding pod; eligibility matters more than “every node without exception.”

## Management
- The controller maintains pods as nodes and configuration change.
- Update strategy determines how pod replacements are rolled out.
- Inspect `kubectl get daemonsets`, the pod selector, and node scheduling events.
- Resource requests still affect node capacity; a node agent is not exempt from resource planning.

Use [[Kubernetes Worker Nodes]] for node components and [[Kubernetes Observability]] for common collector roles.

# References
[[8 - Kubernetes Object Model]]
[DaemonSets](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)
