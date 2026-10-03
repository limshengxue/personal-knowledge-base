2025-12-13 09:35

# 5 - Kubernetes Configuration
## Configuration
- All-in-One single node 
	- For learning, development, testing only
- Single-control plane and Multi-worker
- Single-control plane with Single-node etcd, and Multi-Worker
	- External etcd instance
- Multi-control plane and Multi-worker
	- High Availability (HA) - each control plane node running a stacked etcd instance. Etcd instance also form a HA etcd cluster.
- Multi-control plane with Multi-Node etcd, and Multi-worker
	- Control plane paired with external etcd instance. More advanced cluster configuration.

## Infrastructure
- Once decide the installation type, we need to define the infrastructure
	- Bare metal/public/private/hybrid cloud
	- Underlying OS
	- Which networking solutions

## Production Clusters Deployment Tools
- There are tools to help us bootstrap a production k8s cluster
- `kubeadm` 
	- secure and recommended method to bootstrap multi-node HA cluster, on premise or in the code
	- can also bootstrap single-node cluster for learning
	- does not support host provisioning (assume there is a machine ready for k8s)
- `kubespray`
	- Allow install HA clusters on several cloud providers and bare metal
- `kops`
	- Allow HA cluster installation 
	- Support provision of infrastructure as well
	- AWS and GCE support

# References
