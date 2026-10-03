2026-03-01 09:29

Tags: [[kubernetes]]

# Kubernetes
- Kubernetes is an open-source system for automating deployment, scaling, and management of containerized applications
- Inspired by Google Borg system
- Written in Go

## Kubernetes Features
Kubernetes offers a very rich set of features for container orchestration. Some of its fully supported features are:
- Automatic bin packaging - k8s decide which nodes to run the pods (we define what resources it require)
- Self healing - restart containers that failed health checks
- Service discovery and load balancing - provide IP and DNS to containers
- Horizontal scaling
- Extensibility support by 
	- Various plugins including 3rd party open source tools
	- Custom resources
- Automated rollouts and rollbacks
- Secret and configuration management
- Storage orchestration
- Batch execution
- Ipv4 and v6 support

## Kubernetes Portability
- It can be deployed in many environments such as local or remote Virtual Machines, bare metal, or in public/private/hybrid/multi-cloud setups.
- Once decide the installation type, we need to define the infrastructure
	- Bare metal/public/private/hybrid cloud
	- Underlying OS
	- Which networking solutions

## K8s Configuration
- All-in-One single node 
	- For learning, development, testing only
- Single-control plane and Multi-worker
- Single-control plane with Single-node etcd, and Multi-Worker
	- External etcd instance
- Multi-control plane and Multi-worker
	- High Availability (HA) - each control plane node running a stacked etcd instance. Etcd instance also form a HA etcd cluster.
- Multi-control plane with Multi-Node etcd, and Multi-worker
	- Control plane paired with external etcd instance. More advanced cluster configuration.

## High-level Architecture
- At high level, k8s is a cluster of compute systems categorized by distinct roles
	- 1 or more control plane nodes [[Kubernetes Control Plane]]
	- 1 or more worker nodes [[Kubernetes Worker Nodes]]
![[Attachments/Pasted image 20251130093836.png]]


# References
[[3 - Kubernetes]]
[[5 - Kubernetes Configuration]]
