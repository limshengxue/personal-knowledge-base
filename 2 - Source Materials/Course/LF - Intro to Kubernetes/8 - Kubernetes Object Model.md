2025-12-13 11:29

# 8 - Kubernetes Object Model
- Object model representing different persistent entities in the cluster
- They describe
	- What containerized applications we are running
	- The nodes where the applications are deploy
	- Application resource consumptions
	- Policies attached to the application (like restart, upgrade, fault tolerance)
- For each object, we declare desired state in `spec`
- Where `status` record the actual state
- The object definition manifest include `apiVersion` and `kind`
- We often use YAML to define, which convert by kubectl to JSON and sent to the API server


## Nodes
- Virtual identities assigned by k8s to the systems part of the cluster
- Can be VM, bare metal, containers
- Unique to each system
- Used by cluster for resources accounting
- Each node host a container runtime
- Nodes are managed by node agents (kubelet and kube-proxy) [[4 - Kubernetes Architecture#Worker Node Components]]
- Nodes can be control plane or workers
- Single all-in-one is a special case where single node run both control plane and workers
- Node identities are created and assigned during cluster bootstrapping process
	- Minikube is using the default kubeadm bootstrapping tool, to initialize the control plane node during the _init_ phase and grow the cluster by adding worker or control plane nodes with the _join_ phase.

## Namespaces
- Virtual sub-clusters to fulfill multiple users and teams
- Names of resources/objects in a namespace is unique but not across namespaces
- Generally, Kubernetes creates four Namespaces out of the box
	-  **kube-system** Namespace contains the objects created by the Kubernetes system, mostly the control plane agents. 
	- The **default** Namespace contains the objects and resources created by administrators and developers, and objects are assigned to it by default unless another Namespace name is provided by the user. 
	- **kube-public** is a special Namespace, which is unsecured and readable by anyone, used for special purposes such as exposing public (non-sensitive) information about the cluster. 
	- The newest Namespace is **kube-node-lease** which holds node lease objects used for node heartbeat data. 
-  Good practice, however, is to create additional Namespaces, as desired, to virtualize the cluster and isolate users, developer teams, applications, or tiers

## Pod
- Smallest k8s workload object
- Unit of deployment
- Single instance of the application
- Logical collection of 1 or more containers, enclosing and isolating them to ensure they
	- Are scheduled together on the same host
	- Share the same network namespace
	- Have access to mount the same volumes
- They are ephemeral (temporary and disposable). Thus, they are used with controller to handle like replication, self-healing, fault tolerance, etc
- Generally we do not deploy pod directly, but use some type of an operator to run and manage Pods
- Example of pod definition
```yaml
**apiVersion: v1**  
**kind: Pod**  
**metadata:**  
  **name: nginx-pod**  
  **labels:**  
    **run: nginx-pod**  
**spec:**  
  **containers:**  
  **- name: nginx-pod**  
    **image: nginx:1.22.1**  
    **ports:**  
    **- containerPort: 80**
```

### Run a Pod immediately
`kubectl run nginx-pod --image=nginx:1.22.1 --port=80`

### Generate a Pod manifest (without creating the Pod)
#### YAML manifest
```bash
kubectl run nginx-pod --image=nginx:1.22.1 --port=80 \
  --dry-run=client -o yaml > nginx-pod.yaml
```
#### JSON manifest
```bash
kubectl run nginx-pod --image=nginx:1.22.1 --port=80 \
  --dry-run=client -o json > nginx-pod.json
```
These files can be used as **templates** or applied to the cluster.

### Create the Pod from a manifest
```bash
kubectl create -f nginx-pod.yaml
kubectl create -f nginx-pod.json
```

### Common Pod management commands
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

## Label
- Key-value pairs attached to k8s objects
- Used to organize and select a subset of objects
- We select label using label selectors
	- Equality-based
	- Set-based operator like `in`, `notin` for values and `exist` `notexist` for keys

## Replication Controllers
- Ensure specific number of replicas of a pod are running at any given time 
- However, Replication Controller it not recommended, instead we use Deployment which configure ReplicaSet controller

### ReplicaSet
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


## Deployment
- Part of the control plane node's controller manager
- Allows seamless application updates and rollbacks, known as the default RollingUpdate strategy through rollouts and rollbacks
- Directly manages ReplicaSets
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