2026-03-01 10:08

Tags: [[kubernetes]] [[Kubernetes Object Model]]

# Kubernetes Deployment
- A workload API object reconciled by the Deployment controller.
- Uses RollingUpdate by default to replace replicas gradually; readiness checks and sufficient capacity help preserve availability, but updates and rollbacks are not automatically downtime-free.
- Directly manages ReplicaSets
- The common management hierarchy is 
	- Deployment -> ReplicaSets [[Kubernetes ReplicaSet]] -> Pod [[Kubernetes Pod]]
- We rarely directly used ReplicaSet or Pod in production environment, we use deployment

```bash
# Check rollout status
kubectl rollout status deploy nginx-deployment

## View rollout history
kubectl rollout history deploy nginx-deployment

## View specific rollout history
kubectl rollout history deploy nginx-deployment --revision=1

## Update container image
kubectl set image deploy nginx-deployment nginx=nginx:stable

## Rollback to previous revision
kubectl rollout undo deploy nginx-deployment --to-revision=1
```

## Related Workload
Use [[Kubernetes DaemonSets]] when the desired placement is one pod per eligible node rather than a global replica count.

## Application Deployment Strategies
- Rolling update gradually replaces replicas; configure readiness checks and rollout bounds.
- Canary runs old/new releases together and sends selected traffic to the new release. Replica ratios can approximate traffic share, but do not guarantee request-level weights.
- Blue/green maintains separate old/new environments and switches traffic after verification; the inactive version is not merely a backup pod.
- Explicit weighted routing may require a gateway or [[Kubernetes Service Mesh]].
- Rollback must consider database/schema compatibility and external effects, not only container images.

The image update command is illustrative. Pin an approved version or digest in real deployments, and verify readiness with [[Kubernetes Liveness and Probe]] before judging a rollout healthy.

# References
[[8 - Kubernetes Object Model]]
