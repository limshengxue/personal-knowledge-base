2026-03-01 09:52

Tags: [[3 - Tags/kubernetes]]

# Minikube
- Most easiest, flexible way to run all-in-one or multi-node local k8s cluster
- The selected driver supplies a VM, container, or bare-metal environment. A type-2 hypervisor is not universally required; requirements differ by driver and operating system.
- Prefer an isolated VM or container driver for a disposable learning cluster. The none driver operates on the host and requires additional care.
- Build on `libmachine` originally designed by Docker to build VM container hosts
- Minikube invokes `kubeadm` to bootstrap cluster components

## Starting Minikube -`minikube start`
- Download latest k8s components
- Run using selected driver to provision a VM or container to host the cluster
- Once the node provisioned, bootstraps the K8s control plane, install container runtime
- start a default cluster (minikube)
- `minikube profile` - view the status of all clusters
- profile is a features that allow creation of reusable cluster

## Active Cluster
- active profile is the cluster which the command will be applied to 
- active kubecontext is the cluster which kubectl operates on
![[Attachments/Pasted image 20251213102459.png]]

## Accessing Minikube
### CTL Tools `kubectl`
- Allow manage local or remote cluster
- minikube comes with kubectl but only as subcommand, it is better to install a standalone

#### kubectl configuration file
- client need control plane node endpoint and credentials to interact with API server
- When using with minikube, minikube by default create the config file in home directory and kubectl go and get this file.

### Minikube Dashboard
```bash
$ minikube addons enable metrics-server
$ minikube addons enable dashboard
$ minikube dashboard
```

### API
#### With kubectl proxy
- We can run `kubectl proxy` to authenticates with an API server on control plane and makes it listen to proxy port (default 8001)

#### With authentication
- Can also use bearer token, or a set of keys and certificates


## Deploy and Expose a Local Application
An example `deployment.yaml` for the source's nginx application:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webserver
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:alpine
          ports:
            - containerPort: 80
```

From the directory containing that manifest, with the intended Minikube context active:

```bash
kubectl apply -f deployment.yaml
kubectl rollout status deployment/webserver
kubectl get pods -L app
kubectl get pods -l app=nginx -o wide
kubectl expose deployment webserver --name=web-lb --type=LoadBalancer --port=8080 --target-port=80
```

- `-L app` displays a label column; `-l app=nginx` filters by that label.
- The Deployment selector must match its pod-template labels.
- The generated [[Kubernetes Service]] selects the deployment's pods.
- Service port 8080 forwards to container port 80; omitting the correct target port can leave a service unable to reach nginx.

## Tunnel and Verify
- Run `minikube tunnel` in a separate terminal and keep it open while using the LoadBalancer.
- Inspect `kubectl get services` and `kubectl describe service web-lb`.
- Access the assigned external address on port 8080.
- A pending external IP can indicate that the local tunnel is not running.
- NodePort access can use `minikube service <service-name> --url`; reachability and tunnel requirements depend on the driver and operating system. [Minikube application access](https://minikube.sigs.k8s.io/docs/handbook/accessing/).

This is a local learning manifest, not a production deployment configuration.

# References
[[2 - Source Materials/Course/LF - Intro to Kubernetes/6 - Minikube|6 - Minikube]]
[[2 - Source Materials/Course/LF - Intro to Kubernetes/7 - Accessing Minikube|7 - Accessing Minikube]]
[[2 - Source Materials/Course/LF - Intro to Kubernetes/12 - Deploying App E2E minikube|12 - Deploying App E2E minikube]]
[Supported drivers](https://minikube.sigs.k8s.io/docs/drivers/)
