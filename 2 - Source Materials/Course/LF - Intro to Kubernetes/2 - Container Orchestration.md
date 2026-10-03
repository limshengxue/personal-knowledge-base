2025-11-30 09:02

# 2 - Container Orchestration
- Container images allow confine application code, its runtime, and all dependencies in a pre-defined format
- Container runtime use pre-packaged image as source to create and run one or more containers
- These runtimes are capable of running multiple containers on a single host
- However, for fault-tolerant and scalable solution, we would like to have single controller/management unit, a collection of multiple host connected together
- This unit is referred to as container orchestrator

## Containers
- Application-centric method to deliver high-performing, scalable application on any infrastructure
- Portable, isolated virtual environment for application to run
- Container run images
- Image bundles the application along with its runtime, libraries, and dependencies

## Container Orchestration
- In Dev environment, running container on single host may be suitable
- But not for QA and Prod environment for several reasons
	- Fault tolerance
	- On-demand scalability
	- Optimal resource usage
	- Auto-discovery to automatically discover and communicate with each other
	- Accessibility from the outside world
	- Seamless updates/rollbacks without any downtime.
- Container orchestrators are tools which group systems together to form clusters
- Containers' deployment and management is automated at scale

### Why use Container Orchestrators?
Most container orchestrators can:
- Group hosts together while creating a cluster, in order to leverage the benefits of distributed systems.
- Schedule containers to run on hosts in the cluster based on resources availability.
- Enable containers in a cluster to communicate with each other regardless of the host they are deployed to in the cluster.
- Bind containers and storage resources.
- Group sets of similar containers and bind them to load-balancing constructs to simplify access to containerized applications by creating an interface, a level of abstraction between the containers and the client.
- Manage and optimize resource usage.
- Allow for implementation of policies to secure access to applications running inside containers.

# Where to Deploy Container Orchestrators?
Most container orchestrators can be deployed on the infrastructure of our choice - on bare metal, Virtual Machines, on-premises, on public and hybrid clouds. Kubernetes, for example, can be deployed on a workstation, with or without an isolation layer such as a local hypervisor or container runtime, inside a company's data center, in the cloud on AWS Elastic Compute Cloud (EC2) instances, Google Compute Engine (GCE) VMs, DigitalOcean Droplets, IBM Virtual Servers, OpenStack, etc.

In addition, there are turnkey cloud solutions which allow production Kubernetes clusters to be installed, with only a few commands, on top of cloud Infrastructures-as-a-Service. These solutions paved the way for the managed container orchestration as-a-Service, more specifically the managed Kubernetes as-a-Service (KaaS) solution, offered and hosted by the major cloud providers. Examples of KaaS solutions are [Amazon Elastic Kubernetes Service](https://aws.amazon.com/eks/) (Amazon EKS), [Azure Kubernetes Service](https://azure.microsoft.com/en-us/products/kubernetes-service) (AKS), [DigitalOcean Kubernetes](https://www.digitalocean.com/products/kubernetes), [Google Kubernetes Engine](https://cloud.google.com/kubernetes-engine/) (GKE), [IBM Cloud Kubernetes Service](https://www.ibm.com/products/kubernetes-service), [Oracle Container Engine for Kubernetes](https://www.oracle.com/cloud/cloud-native/container-engine-kubernetes/), or [VMware Tanzu Kubernetes Grid](https://tanzu.vmware.com/kubernetes-grid).


# References
