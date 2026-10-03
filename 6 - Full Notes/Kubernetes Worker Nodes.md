2026-03-01 09:40

Tags: [[kubernetes]] 

# Kubernetes Worker Nodes
Provide RTE for client applications which are containers
- Containers are encapsulated in Pods, controlled by cluster control plane agents
- *Pods are scheduled on worker nodes* [[Kubernetes Pod]]
- In a *multi-worker Kubernetes cluster*, the *network traffic* between client users and the containerized applications deployed in Pods is *handled directly by the worker nodes*, and is not routed through the control plane node.


## Worker Node Components
### Container Runtime
- Runs Pod containers through CRI-compatible runtimes such as containerd or CRI-O. Docker Engine requires an adapter such as cri-dockerd; it does not implement CRI directly.

### Node agent - kubelet
- Agent running on each node (control plane and workers)
- Communicate with control plane
- Receive Pod definitions, interacts with container runtime on the node to run containers of the Pod
- It also monitors the health and resources of the Pods running containers
- Container Runtime Interface (CRI) - connect to inter-changeable container runtime
- CRI implements 2 services
	- ImageService - responsible for all image-related operations
	- RuntimeService - responsible for all Pod and container-related operations

![[Attachments/Pasted image 20251130102913.png]]


### Kubelet  - CRI shims
- CRI is an effort introduce to support more container runtime without the need to change kubelet's source code
- Any container runtime implement CRI can be supported
- Shims are CRI implementations, interfaces, or adapters, specific to each container runtime supported by k8s
- containerd's CRI plugin provides Kubernetes integration
- CRI-O - support Open Container Initiative (OCI) compatible runtime with k8s
- Built-in dockershim was removed in Kubernetes 1.24. cri-dockerd is an external adapter for Docker Engine, not the built-in shim.

### Proxy - kube-proxy 
- Network agent which runs on each node (control plane and workers)
- Responsible for dynamic updates and maintenance of all networking rules
- Abstract details of Pods networking and forwards connection requests to the containers in the Pods
- Programs forwarding rules using a supported backend such as iptables or nftables. Some networking implementations replace kube-proxy; it is not mandatory in every cluster.
- [[Kubernetes Service#Kube-proxy]]

### Add-ons
Add-ons extend a cluster with supporting capabilities; they may be first-party or third-party components, and are not all absent from the Kubernetes ecosystem itself.
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


# References
[[4 - Kubernetes Architecture]]
[Dockershim removal FAQ](https://kubernetes.io/blog/2022/02/17/dockershim-faq/)
