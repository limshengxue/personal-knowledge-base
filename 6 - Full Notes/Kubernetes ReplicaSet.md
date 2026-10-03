2026-03-01 10:05

Tags: [[kubernetes]] [[Kubernetes Object Model]]

# Kubernetes ReplicaSet
- Next generation of Replication Controller
- Implements replication and self-healing
- We can scale number of pods running  manually or using autoscaler
- An example manifest for ReplicaSet

```yaml
# API version used for ReplicaSet resources
apiVersion: apps/v1

# Kind of Kubernetes object being created
kind: ReplicaSet

metadata:
  # Name of the ReplicaSet
  name: frontend

  # Labels attached to the ReplicaSet object itself
  labels:
    app: guestbook
    tier: frontend

spec:
  # Desired number of Pod replicas
  replicas: 3

  # Selector tells the ReplicaSet which Pods it manages
  # It must match the labels defined in the Pod template
  selector:
    matchLabels:
      app: guestbook

  # Pod template used to create Pods
  template:
    metadata:
      # Labels applied to Pods created by this ReplicaSet
      labels:
        app: guestbook

    spec:
      # List of containers running inside each Pod
      containers:
      - name: php-redis              # Container name
        image: gcr.io/google_samples/gb-frontend:v3  # Container image

```

## Managing ReplicaSet
### Create a ReplicaSet
Loads the ReplicaSet manifest and creates the specified number of Pod replicas:
```bash
kubectl create -f redis-rs.yaml
```

### Update or reapply the ReplicaSet
`kubectl apply -f redis-rs.yaml`

### View ReplicaSets
`kubectl get replicasets kubectl get rs`

### Scale a ReplicaSet
Change the number of running Pod replicas:
`kubectl scale rs frontend --replicas=4`

### Inspect ReplicaSet details
```bash
kubectl get rs frontend -o yaml 
kubectl get rs frontend -o json 
kubectl describe rs frontend
```

### Delete a ReplicaSet
`kubectl delete rs frontend`


# References
[[8 - Kubernetes Object Model]]