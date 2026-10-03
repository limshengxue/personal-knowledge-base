2026-03-01 10:43

Tags: [[kubernetes]]

# Kubernetes Advanced Concepts
## Annotations
- Attach arbitrary, non-identifying metadata to any object, in key-value format
- Cannot be used to search object like "Label"

## Quota and Limits Management
- `ResourceQuota` API can be used to constraint resource consumption per Namespace
- Can limit
	- Compute Resource
	- Storage Resource
	- Object Count
- `LimitRange` used to impose limit on Resource Level
	- Compute Resource per pod/container
	- Storage request limit per PVC
	- Limit ratio for a resource
- Autoscaling implemented to adjust number of running objects or resources based on metrics
	- Horizontal Pod Autoscaler (HPA) - adjust number of replica
	- Vertical Pod Autoscaler - adjust resource requirement
	- Cluster Autoscaler - scale the cluster

## Jobs and CronJobds
- Job create 1 or more Pods to perform a given task
- Once task completed, the pods will be terminated
- We can configure Jobs
	- Paralleism
	- Completions
	- activeDeadlineSeconds
	- backOffLimit - number of retry before report as failed
	- ttlSecondsAfterFinished
- CronJobs provide better scheduling
	- startingDeadlineSeconds - deadline to start the job if scheduled time missed
	- concurrencyPolicy

## StatefulSets
- Used for stateful applications which require unique identity
- Eg. MySQL cluster

## Custom Resource
- Resource is an API endpoint that stores a collection of API objects
- Pod resource contains all Pod objects
- We can create custom resources if the existing doesnt fulfill the requirements
- 2 ways
	- Custom Resource Definition
	- API Aggregation - sit behind primary API server, serve request forwarded by primary API server
- To have custom resource, we must create and install custom controller


## Security Contexts and Pod Security Admission
- Define privileges and access control settings for Pods & Containers
- We can define using Pod manifest - but limited to 1 pod only
- Pod security admission enable multiple Pods and Containers cluster-wide

## Network Policies
- Define rules that control how pods communicate
- Define Ingress (inbound) and Egress (outbound) traffic for a certain set of protected rules
- Can be define based on label, namespace, port, or IP

## Monitoring, Logging, and Troubleshooting
- We collect resource usage data by Pods, Services, nodes etc
- 2 popular solutions
	- Kubernetes Metrics Server - new features of k8s as a plugin, cluster-wide aggregator of resource usage data. 
	- Prometheus - scrape the resource usage of different components and object
- K8s does not provide another support for logging
- The default support view log of current running container and last failed only
- We can view cluster event using
	- `kubectl get events`, or `kubectl describe pod <pod-name>`
- Common logging solution is Elasticsearch with Fluentd

## Helm
- We need multiple k8s resources for a complex application
- Having separated manifest and deploying them one-by-one is a trouble
- We can combine them into 1 well-defined format known as Chart
- These charts can then be served and managed using package manager like Helm

## Service Mesh
- 3rd party solution alternative to K8s native app connectivity and exposure achieved with Services and Ingress Controllers
- Introduce features like service discovery, mutual TLS, multi-cloud routing, and traffic telemetry
- An implementation that relies on a proxy component part of the Data Plane which is then managed through the Control Plane

## Application Deployment Strategies
- Default: Rolling Update, might not fulfill all scenario
- Canary: runs 2 app release simultaneously managed by 2 independent Deployment, user traffic is routed to either one which probability can be controlled by scaling
- Blue/green: runs 2 simultaneously, but 1 act as backup only


# References
[[17 - Advanced Concepts]]