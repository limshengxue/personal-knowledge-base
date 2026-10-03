2026-03-01 10:03

Tags: [[Kubernetes Object Model]] [[kubernetes]]

# Kubernetes Pod
- Smallest k8s workload object
- Unit of deployment
- A co-scheduled set of containers, not necessarily an entire application
- Logical collection of 1 or more containers, enclosing and isolating them to ensure they
	- Are scheduled together on the same host
	- Share the same network namespace
	- Have access to mount the same volumes
- They are ephemeral (temporary and disposable). Thus, they are used with controller to handle like replication, self-healing, fault tolerance, etc
- Normally use a built-in workload controller, such as [[Kubernetes Deployment]] or [[Kubernetes StatefulSets]], rather than managing standalone Pods. Operators are a separate extension pattern.
- Example of pod definition
```yaml
apiVersion: v1  
kind: Pod  
metadata:  
  name: nginx-pod  
  labels:  
    run: nginx-pod  
spec:  
  containers:  
  - name: nginx-pod  
    image: nginx:stable  
    ports:  
    - containerPort: 80
```

## Run a Pod immediately
`kubectl run nginx-pod --image=nginx:stable --port=80`

## Generate a Pod manifest (without creating the Pod)
### YAML manifest
```bash
kubectl run nginx-pod --image=nginx:stable --port=80 \
  --dry-run=client -o yaml > nginx-pod.yaml
```
### JSON manifest
```bash
kubectl run nginx-pod --image=nginx:stable --port=80 \
  --dry-run=client -o json > nginx-pod.json
```
These files can be used as templates or applied to the cluster.

## Create the Pod from a manifest
```bash
kubectl create -f nginx-pod.yaml
kubectl create -f nginx-pod.json
```

## Common Pod management commands
```bash
kubectl apply -f nginx-pod.yaml
kubectl get pods
kubectl get pod nginx-pod -o yaml
kubectl get pod nginx-pod -o json
kubectl describe pod nginx-pod
kubectl delete pod nginx-pod
```

### Key idea
- `kubectl run` → imperative (quick start)
- `--dry-run -o yaml/json` → generate reusable manifests
- `create / apply` → declarative management



Use [[Kubernetes Liveness and Probe|health probes]] for readiness and restart decisions. The example image tag is for learning; pin an approved version or digest for reproducible deployments.

# References
[[8 - Kubernetes Object Model]]
