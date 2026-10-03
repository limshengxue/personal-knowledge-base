2026-03-01 09:40

Tags: [[kubernetes]] 

# Kubernetes Worker Nodes
Provide RTE for client applications which are containers
- Containers are encapsulated in Pods, controlled by cluster control plane agents
- *Pods are scheduled on worker nodes* [[Kubernetes Pod]]
- In a *multi-worker Kubernetes cluster*, the *network traffic* between client users and the containerized applications deployed in Pods is *handled directly by the worker nodes*, and is not routed through the control plane node.


## Worker Node Components
### Container Runtime
- Stay on the node to run the containers of pod, e.g. Docker Engine

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
- cri-containerd - support containerd
- CRI-O - support Open Container Initiative (OCI) compatible runtime with k8s
- dockershim and cri-dockerd (new)

### Proxy - kube-proxy 
- Network agent which runs on each node (control plane and workers)
- Responsible for dynamic updates and maintenance of all networking rules
- Abstract details of Pods networking and forwards connection requests to the containers in the Pods
- Operates in conjunction with iptables of the node
- [[Kubernetes Service#Kube-proxy]]

### Add-ons
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


# References
[[4 - Kubernetes Architecture]]
