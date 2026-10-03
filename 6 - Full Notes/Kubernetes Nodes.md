2026-03-01 09:59

Tags: [[kubernetes]] [[Kubernetes Object Model]]

# Kubernetes Nodes
- Virtual identities assigned by k8s to the systems part of the cluster
- Can be VM, bare metal, containers
- Unique to each system
- Used by cluster for resources accounting
- Each node host a container runtime
- Nodes are managed by node agents (kubelet and kube-proxy) 
- Nodes can be [[Kubernetes Control Plane]] or [[Kubernetes Worker Nodes]]
- Single all-in-one is a special case where single node run both control plane and workers (eg. [[Minikube]])
- Node identities are created and assigned during cluster bootstrapping process
	- Minikube is using the default kubeadm bootstrapping tool, to initialize the control plane node during the _init_ phase and grow the cluster by adding worker or control plane nodes with the _join_ phase.


# References
[[8 - Kubernetes Object Model]]