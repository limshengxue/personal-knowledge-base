2026-10-03 18:49

Tags: [[3 - Tags/kubernetes|kubernetes]]

# Kubernetes Service Mesh
- A service mesh adds application-traffic controls such as identity, mutual TLS, telemetry, and routing.
- It complements the Kubernetes connectivity model rather than universally replacing [[Kubernetes Service]] or [[Kubernetes Ingress]].

## Common Architecture
- A control plane distributes policies and configuration.
- Data-plane components enforce supported behavior and observe traffic.
- Implementations can use different proxy deployment models; a per-pod sidecar is not universal.

## Possible Uses
- Consistent service-to-service authentication.
- Request-level telemetry and supported routing policies.
- Weighted traffic shifts for application rollouts.
- Multi-cluster connectivity where supported and intentionally configured.

## Costs and Limits
- Adds operational complexity, resource costs, and another failure surface.
- Does not repair application-level business logic or automatically make every dependency reliable.
- Check implementation support, rollout strategy, certificate lifecycle, and exclusions.
- Use [[Kubernetes Networking]] as the underlying model and [[Kubernetes Deployment]] for rollout responsibilities.

# References
[[17 - Advanced Concepts]]
[Service mesh concepts](https://istio.io/latest/about/service-mesh/)
