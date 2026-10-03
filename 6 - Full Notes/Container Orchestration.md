2026-03-01 09:23

Tags: [[3 - Tags/kubernetes]] [[container]] [[docker]]

# Container Orchestration
## Containers
- Application-centric method to deliver high-performing, scalable application on any infrastructure
- Portable, isolated virtual environment for application to run
- Container run images
- Image bundles the application along with its runtime, libraries, and dependencies

### Container Runtime
- Container runtime like Docker use pre-packaged image as source to create and run one or more containers
- These runtimes are capable of running multiple containers on a single host
- However, for fault-tolerant and scalable solution, we would like to have single controller/management unit, a collection of multiple host connected together
- This unit is referred to as container orchestrator

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


# References