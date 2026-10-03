2026-03-01 10:08

Tags: [[kubernetes]] [[Kubernetes Object Model]]

# Kubernetes Deployment
- Part of the control plane node's controller manager
- Allows seamless application updates and rollbacks, known as the default RollingUpdate strategy through rollouts and rollbacks
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
kubectl set image deploy nginx-deployment nginx=nginx:1.21.5

## Rollback to previous revision
kubectl rollout undo deploy nginx-deployment --to-revision=1
```

## DaemonSets
- Works similar as Deployment with 1 distinct features: ensure 1 pod per nodes
- Run on all nodes or selected subset
- Useful for daemon that run program like monitoring
- Whenever a Node added to the cluster, a Pod from the DaemonSet placed on it


# References
[[8 - Kubernetes Object Model]]