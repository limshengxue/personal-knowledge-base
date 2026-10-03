2026-03-01 09:52

Tags: [[3 - Tags/kubernetes]]

# Minikube
- Most easiest, flexible way to run all-in-one or multi-node local k8s cluster
- Type-2 hypervisor is required to offer an isolated infrastructure of the cluster components
- Isolation is require to ensure proper tear down
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
- We can run `**kubectl proxy**` to authenticates with an API server on control plane and makes it listen to proxy port (default 8001)

#### With authentication
- Can also use bearer token, or a set of keys and certificates


# References
[[6 - Minikube]]
[[7 - Accessing Minikube]]
