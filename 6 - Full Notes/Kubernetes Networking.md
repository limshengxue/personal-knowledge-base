2026-03-01 09:50

Tags: [[kubernetes]]

# Kubernetes Networking
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
[[4 - Kubernetes Architecture]]