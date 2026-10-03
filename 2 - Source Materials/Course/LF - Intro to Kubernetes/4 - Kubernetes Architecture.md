2025-11-30 09:37

# 4 - Kubernetes Architecture
- At high level, k8s is a cluster of compute systems categorized by distinct roles
	- 1 or more control plane nodes
	- 1 or more worker nodes
![[Attachments/Pasted image 20251130093836.png]]


## Control Plane Node
- Provides RTE for *control plane agents* responsible for managing the state of the cluster
- The agents are the brain behind all operations inside the cluster
- To communicate with the cluster, user send requests to the control plane via CLI, Web UI, or API
- Control plane node must be *kept running at all cost*
- Losing it brings service disruption
- *Control plan node replicas* can be added to ensure cluster running in HA (high availability) mode
- To persists state, cluster config data is saved to distributed key-value store either in
	- Control plane node (stacked topology)
	- Or dedicated host (external topology)
- In stacked topology, control plane node replica ensure key-value stores resiliency as well

### Control Plane Node Components
A control plane node runs the following essential control plane components and agents: API server, scheduler, controller managers, and key-value data store.

In addition, the control plane node runs: container runtime, node agent (kubelet), proxy (kube-proxy), optional add-ons for observability, such as dashboard, cluster-level monitoring, and logging.

#### API Server
- `kube-apiserver`
- Central control plane component
- *Intercept, validates, process RESTful calls*
- Read and write key-value stores based on request
- The only control plane component that talks to the store (act is middleware to the cluster state)
- Highly configurable and customizable
- Can add a secondary API Servers

#### Scheduler
- *Assign new workload objects (e.g. pods encapsulating containers) to worker nodes*
- Made decision based on cluster state and new workload object requirement
- Both state and workload object requirements are received from API Server
- Take Quality of Service (QoS) requirements into consideration
- The outcome of the decision is sent back to API server

#### Controller Managers
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

#### Key-value Data Store
- `etcd`
- Appending store (data never replaced)
- Obsolete data is compacted periodically
- CLI management tool `etcdctl` - provide snapshot save and restore
- `etcd` replica used Leader-follower model for fault-tolerance
- Written in Go
- *Besides storing cluster state, also store configuration details like subnets, ConfigMaps, Secrets*


## Worker Node
- Provide RTE for client applications which are containers
- Containers are encapsulated in Pods, controlled by cluster control plane agents
- *Pods are scheduled on worker nodes*
- Pod 
	- smallest scheduling work unit in k8s
	- logical collection of 1 or more containers
	- Start, stop, rescheduled as a single unit of work
	- Schedule in 1 node together
- In a *multi-worker Kubernetes cluster*, the *network traffic* between client users and the containerized applications deployed in Pods is *handled directly by the worker nodes*, and is not routed through the control plane node.


### Worker Node Components
#### Container Runtime
- Stay on the node to run the containers of pod, e.g. Docker Engine

#### Node agent - kubelet
- Agent running on each node (control plane and workers)
- Communicate with control plane
- Receive Pod definitions, interacts with container runtime on the node to run containers of the Pod
- It also monitors the health and resources of the Pods running containers
- Container Runtime Interface (CRI) - connect to inter-changeable container runtime
- CRI implements 2 services
	- ImageService - responsible for all image-related operations
	- RuntimeService - responsible for all Pod and container-related operations

![[Attachments/Pasted image 20251130102913.png]]


#### kubelet  - CRI shims
- CRI is an effort introduce to support more container runtime without the need to change kubelet's source code
- Any container runtime implement CRI can be supported
- Shims are CRI implementations, interfaces, or adapters, specific to each container runtime supported by k8s
- cri-containerd - support containerd
- CRI-O - support Open Container Initiative (OCI) compatible runtime with k8s
- dockershim and cri-dockerd (new)

#### Proxy - kube-proxy
- Network agent which runs on each node (control plane and workers)
- Responsible for dynamic updates and maintenance of all networking rules
- Abstract details of Pods networking and forwards connection requests to the containers in the Pods
- Operates in conjunction with iptables of the node

#### Add-ons
Add-ons are cluster features and functionality not yet available in Kubernetes, therefore implemented through 3rd-party plugins and services.
- DNS  
    Cluster DNS is a DNS server required to assign DNS records to Kubernetes objects and resources.
- Dashboard  
    A general purpose web-based user interface for cluster management.
- Monitoring  
    Collects cluster-level container metrics and saves them to a central data store.
- Logging  
    Collects cluster-level container logs and saves them to a central log store for analysis.
- Device Plugins  
    For system hardware resources, such as GPU, FPGA, high-performance NIC, to be advertised by the node to application pods.

## Networking
### Container-to-Container inside Pods
- Shared network namespace across containers
- When a grouping of containers defined by a Pod is started, a special infrastructure Pause container is initialized by the Container Runtime for the sole purpose of creating a network namespace for the Pod. 
- All additional containers, created through user requests, running inside the Pod will share the Pause container's network namespace so that they can all talk to each other via localhost.

### Pod-to-Pod across nodes
- K8s treat pods as VM which each Pod receive a unique IP address
- This model called "IP-per-Pod"
- Containers inside the pod must coordinate ports assignment like applications behind a VM
- Container Network Interface (CNI)
	- Allow plugins to configure networking for containers

### External-to-Pod
Kubernetes enables external accessibility through Services, complex encapsulations of network routing rule definitions stored in iptables on cluster nodes and implemented by kube-proxy agents. By exposing services to the external world with the aid of kube-proxy, applications become accessible from outside the cluster over a virtual IP address and a dedicated port number.

```
PUBLIC INTERNET
       |
       v
[ LoadBalancer Public IP ]  ← Public IP
       |
       v
[ Service ] (virtual IP - internal)
       |
       v
[ Pods ] (real Pod IPs)

```



# References
