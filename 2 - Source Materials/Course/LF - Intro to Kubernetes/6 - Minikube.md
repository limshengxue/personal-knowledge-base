2025-12-13 09:50

# 6 - Minikube
- Most easiest, flexible way to run all-in-one or multi-node local k8s cluster
- Type-2 hypervisor is required to offer an isolated infrastructure of the cluster components
- Isolation is require to ensure proper tear down
- Build on `libmachine` originally designed by Docker to build VM container hosts
- Minikube invokes `kubeadm` to bootstrap cluster components

## `minikube start`
- Download latest k8s components
- Run using selected driver to provision a VM or container to host the cluster
- Once the node provisioned, bootstraps the K8s control plane, install container runtime
- start a default cluster (minikube)
- `minikube profile` - view the status of all clusters
- profile is a features that allow creation of reusable cluster

### other variations to start a cluster
```
**$** **minikube start --kubernetes-version=v1.27.10 \**  
  **--driver=podman --profile minipod**

**$ minikube start --nodes=2 --kubernetes-version=v1.28.1 \**  
  **--driver=docker --profile doubledocker**

**$ minikube start --driver=virtualbox --nodes=3 --disk-size=10g \**  
  **--cpus=2 --memory=6g --kubernetes-version=v1.27.12 --cni=calico \**  
  **--container-runtime=cri-o -p multivbox**

**$ minikube start --driver=docker --cpus=6 --memory=8g \**  
  **--kubernetes-version="1.27.12" -p largedock**

**$ minikube start --driver=virtualbox -n 3 --container-runtime=containerd \**  
  **--cni=calico -p minibox**
```
- `--driver` specify the isolation driver used
- `--container-runtime` specify the container runtime
- `--profile` allow creation of profile with a profile name, once profile exist, we no need start the cluster with the specs but only the profile name
- `-cni` specify the container network interface

### active cluster
- active profile is the cluster which the command will be applied to 
- active kubecontext is the cluster which kubectl operates on
![[Attachments/Pasted image 20251213102459.png]]

# References
