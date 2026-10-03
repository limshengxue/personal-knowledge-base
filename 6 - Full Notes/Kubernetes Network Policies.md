2026-10-03 18:49

Tags: [[3 - Tags/kubernetes|kubernetes]]

# Kubernetes Network Policies
- NetworkPolicy controls allowed ingress and egress for selected pods.
- Enforcement requires a network implementation supporting these policies; creating a manifest alone is insufficient.

## Selection and Isolation
- Select pods in a namespace using labels.
- Rules can match pod selectors, namespace selectors, IP blocks, and ports.
- Pods are normally non-isolated for a direction until a policy selecting them establishes isolation for that direction.
- Applicable allow rules combine; order is not a firewall priority list.
- For a connection between isolated peers, the relevant source egress and destination ingress permissions must both permit it.

## Practical Checks
- Verify the CNI's support and actual enforcement.
- Account for DNS and required dependencies when restricting egress.
- Test both allowed and denied connections from representative pods.
- Do not confuse a policy's ingress direction with the HTTP routing resource [[Kubernetes Ingress]].

See [[Kubernetes Networking]] for the traffic model and [[Kubernetes Namespaces]] for namespace selection.

# References
[[17 - Advanced Concepts]]
[NetworkPolicy semantics](https://kubernetes.io/docs/concepts/services-networking/network-policies/)
