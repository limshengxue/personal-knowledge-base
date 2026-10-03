2025-11-30 09:15

# 3 - Kubernetes
## What is Kubernetes
- Kubernetes is an open-source system for automating deployment, scaling, and management of containerized applications
- Inspired by Google Borg system
- Written in Go

## Kubernetes Features
Kubernetes offers a very rich set of features for container orchestration. Some of its fully supported features are:
- Automatic bin packaging - k8s decide which nodes to run the pods (we define what resources it require)
- Self healing - restart containers that failed health checks
- Service discovery and load balancing - provide IP and DNS to containers
- Horizontal scaling
- Extensibility (e.g. logging via plugins)
- Automated rollouts and rollbacks
- Secret and configuration management
- Storage orchestration
- Batch execution
- Ipv4 and v6 support

## Why Use Kubernetes?
Another one of Kubernetes' strengths is portability. It can be deployed in many environments such as local or remote Virtual Machines, bare metal, or in public/private/hybrid/multi-cloud setups.

Kubernetes extensibility allows it to support and to be supported by many 3rd party open source tools which enhance Kubernetes' capabilities and provide a feature-rich experience to its users. It's architecture is modular and pluggable. Not only does it orchestrate modular, decoupled microservices type applications, but also its architecture follows decoupled microservices patterns. Kubernetes' functionality can be extended by writing custom resources, operators, custom APIs, scheduling rules or plugins.

# References
