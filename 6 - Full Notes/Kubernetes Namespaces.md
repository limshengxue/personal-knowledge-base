2026-03-01 10:02

Tags: [[kubernetes]] [[Kubernetes Object Model]]

# Kubernetes Namespaces
- Virtual sub-clusters to fulfill multiple users and teams
- Names of resources/objects in a namespace is unique but not across namespaces
- Generally, Kubernetes creates four Namespaces out of the box
	-  **kube-system** Namespace contains the objects created by the Kubernetes system, mostly the control plane agents. 
	- The **default** Namespace contains the objects and resources created by administrators and developers, and objects are assigned to it by default unless another Namespace name is provided by the user. 
	- **kube-public** is a special Namespace, which is unsecured and readable by anyone, used for special purposes such as exposing public (non-sensitive) information about the cluster. 
	- The newest Namespace is **kube-node-lease** which holds node lease objects used for node heartbeat data. 
-  Good practice, however, is to create additional Namespaces, as desired, to virtualize the cluster and isolate users, developer teams, applications, or tiers


# References
[[8 - Kubernetes Object Model]]