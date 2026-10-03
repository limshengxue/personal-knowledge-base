2026-03-01 09:35

Tags: [[kubernetes]]

# Kubernetes Control Plane
- Provides RTE for *control plane agents* responsible for managing the state of the cluster
- To communicate with the cluster, user send requests to the control plane via CLI, Web UI, or API
- Control plane node must be *kept running at all cost*
- Losing it brings service disruption
- *Control plan node replicas* can be added to ensure cluster running in HA (high availability) mode
- To persists state, cluster config data is saved to distributed key-value store in node or dedicated host depends on [[Kubernetes Introduction#K8s Configuration]]
	- Control plane node (stacked topology)
	- Or dedicated host (external topology)
- In stacked topology, control plane node replica ensure key-value stores resiliency as well

## Control Plane Node Components
- Control plane node runs the following essential control plane components and agents.
- The agents are the brain behind all operations inside the cluster
- In addition, the control plane node runs: container runtime, node agent (kubelet), proxy (kube-proxy), optional add-ons for observability, such as dashboard, cluster-level monitoring, and logging.

### API Server
- `kube-apiserver`
- Central control plane component
- *Intercept, validates, process RESTful calls*
- Read and write key-value stores based on request
- The only control plane component that talks to the store (act is middleware to the cluster state)
- Highly configurable and customizable
- Can add a secondary API Servers

### Scheduler
- *Assign new workload objects (e.g. pods encapsulating containers) to worker nodes*
- Made decision based on cluster state and new workload object requirement
- Both state and workload object requirements are received from API Server
- Take Quality of Service (QoS) requirements into consideration
- The outcome of the decision is sent back to API server

### Controller Managers
- *Run controllers or operator process to regulate the state of the cluster*
- Controllers are watch-loop processes that run continuously and compare the cluster state with desired state
- Take corrective action when required
-  `kube-controller-manager` 
	- Take action when nodes become unavailable
	- Ensure container pod counts (replicas) are as expected
	- To create endpoints, service accounts, and API access tokens.
-  `cloud-controller-manager` 
	- Interact with underlying cloud infra
	- Volumes, load balancing with cloud service

### Key-Value Data Store
- `etcd`
- Appending store (data never replaced)
- Obsolete data is compacted periodically
- CLI management tool `etcdctl` - provide snapshot save and restore
- `etcd` replica used Leader-follower model for fault-tolerance
- Written in Go
- *Besides storing cluster state, also store configuration details like subnets, ConfigMaps, Secrets*


# References
[[4 - Kubernetes Architecture]]