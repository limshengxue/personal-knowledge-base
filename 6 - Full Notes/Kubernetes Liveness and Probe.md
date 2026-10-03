2026-03-01 10:40

Tags: [[kubernetes]]

# Kubernetes Liveness and Probe
Health probes tell the kubelet how to assess containers in a [[Kubernetes Pod]]. They serve different purposes:
- **Liveness**: repeated failure triggers a container restart according to its restart policy.
- **Readiness**: failure marks the container unready and normally excludes its Pod from [[Kubernetes Service|Service]] traffic; it does not restart the container.
- **Startup**: allows slow initialization. Until it succeeds, liveness and readiness probes do not run.

Supported mechanisms include an exec command, HTTP request, TCP socket, and gRPC health check. gRPC is not a generic RPC call; the application must implement the gRPC health-checking protocol.

## Example Configuration
This fragment belongs under a container definition. The application must create `/tmp/healthy` when healthy; otherwise these checks fail.

```yaml
livenessProbe:
  exec:
    command: ["cat", "/tmp/healthy"]
  initialDelaySeconds: 15
  failureThreshold: 3
  periodSeconds: 5
readinessProbe:
  exec:
    command: ["cat", "/tmp/healthy"]
  initialDelaySeconds: 5
  periodSeconds: 5
```

For slow startup, add a startup probe with a failure threshold and period sized to the expected initialization time. Avoid liveness checks that fail merely because a dependency is temporarily unavailable: unnecessary restarts can worsen an outage.

# References
[[13 - Liveness and Probe]]
[Configure probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)
