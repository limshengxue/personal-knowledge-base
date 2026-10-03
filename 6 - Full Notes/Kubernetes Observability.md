2026-10-03 18:49

Tags: [[3 - Tags/kubernetes|kubernetes]]

# Kubernetes Observability
- Observability helps distinguish workload failures, resource pressure, and cluster-management problems.
- Metrics, logs, and events answer different questions; one tool does not replace all three.

## Metrics
- Metrics Server aggregates resource usage for features such as [[Kubernetes Autoscaling]].
- It is not a full historical monitoring database.
- Prometheus commonly scrapes application and infrastructure metrics into a separate monitoring system.

## Logs and Events
- `kubectl logs <pod>` reads a container's logs; use the container selector for multi-container pods.
- `kubectl logs <pod> --previous` can inspect the prior terminated container instance when available.
- `kubectl get events` and `kubectl describe pod <pod>` show relevant cluster events.
- Kubernetes does not supply a complete durable cluster-wide log backend by itself.

## Retention and Access
- Ship logs to a chosen backend when history must outlive pods/nodes.
- Fluentd with Elasticsearch is one possible stack, not a mandatory architecture.
- Plan storage, retention, labels, and access controls.
- Avoid collecting credentials or private payloads unnecessarily.

# References
[[17 - Advanced Concepts]]
[Logging architecture](https://kubernetes.io/docs/concepts/cluster-administration/logging/)
