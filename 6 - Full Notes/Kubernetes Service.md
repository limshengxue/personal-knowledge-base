2026-03-01 10:31

Tags: [[kubernetes]] [[Kubernetes Object Model]]

# Kubernetes Service
- To access the application, the user or another application need to connect to the Pod
- The problem is Pod is ephemeral (can be disposed, rescheduled) in k8s (IP address allocate to them cannot be static)
- Service is created to solve this problem
- It create a higher-level abstraction, groups Pods and define policy to access them
- This grouping is achieved using Labels and Selectors.
- Service can expose a Pod, ReplicaSets, Deployments, etc
- Even with single Pod, using Service benefit in situation of self-healing

## Operators with Pod with Label
- The label at Deployment level has nothing to do with Service later, it is purely for organization purpose
- `matchLabels` in selector define which pod this Deployment own
- the label at `specs` level define the pod label, it tie to the selector of the deployment and later used by the Service
```yaml
**apiVersion: apps/v1  
kind: Deployment  
metadata:  
  labels:  
    app: frontend  
  name: frontend  
spec:  
  replicas: 3  
  selector:  
    matchLabels:  
      app: frontend  
    template:  
      metadata:  
        labels:  
          app: frontend  
      spec:  
        containers:  
        - image: frontend-application  
        name: frontend-application  
        ports:  
        - containerPort: 5000**
```

### Declaring Service Manifest
Service with 1 port
```yaml
apiVersion: v1
kind: Service
metadata:
  name: frontend-svc # will become DNS name later
spec:
  selector:
    app: frontend # define the pod it points to
ports:
- protocol: TCP
  port: 80 # expose 80
  targetPort: 5000 # target port of the pod in points to

```
Service with multiple ports
```yaml
apiVersion: v1  
kind: Service  
metadata:  
  name: my-service  
spec:  
  selector:  
    app: myapp 
  type: NodePort  
  ports:  
  - name: http  
    protocol: TCP  
    port: 8080  
    targetPort: 80 
    nodePort: 31080  
  - name: https  
    protocol: TCP  
    port: 8443  
    targetPort: 443 
    nodePort: 31443
```
Creating the Service object
`kubectl create -f frontend-svc.yaml`

### Endpoints
- Endpoint is `PodIP:TargetPort` 
- They are created and updated dynamically by the Service
- We can check using `kubectl get svc,ep frontend-svc`


## Kube-proxy
- A daemon that runs on every nodes
- The backbone that allow Service to function
- Watch API server for any Services and Endpoints changes
- How routing works
	- Configure *iptables* rules for routing
	- Capture traffic sent to the service
	- Sent to one of the corresponding endpoints
- As `kube-proxy` runs on every node, 
	- every pod on the cluster is possible to use the service 
	- each node have complete iptable copy
- Traffic policies
	- Local - only use Service within the same node
	- Cluster - can use any Service with a *ready* endpoint (default)
	- Both can set to `internal` or `external`
```yaml
apiVersion: v1  
kind: Service  
metadata:  
  name: frontend-svc  
spec:  
  selector:  
    app: frontend  
  ports:  
    - protocol: TCP  
      port: 80  
      targetPort: 5000  
  internalTrafficPolicy: Local  
  externalTrafficPolicy: Local
```

## Service Discovery
- Environment variable
	- Each newly created pod have env variable to record the IP and port of the Service
- DNS 
	- Use add-on DNS (preferred solution)

## Port Forward
- Forward a local port to an application port (can be deployment, service, or pod container)
- Allow debug application running in a remote cluster
```
$ kubectl port-forward deploy/frontend 8080:5000
$ kubectl port-forward frontend-77cbdf6f79-qsdts 8080:5000 
$ kubectl port-forward svc/frontend-svc 8080:80
```

## Service Type
### ClusterIP
- Default service type
- Service receive a Virtual IP address, known as its Cluster IP
- Only accessible within the cluster

### NodePort
- A high-port, dynamically picked from the default range **30000-32767** is assign to the Service, from all worker nodes
- For example, if 31111 is assigned, any traffic to 31111 of any node will redirect to the service
- We can also specify the exact port number given that it is in the range
- The service is accessible to traffic external from the cluster
- Possibly used with Ingress when there is too many service which cause the need to open many ports and create a mess

```
Client
  ↓
NodeIP:32233
  ↓ (kube-proxy rules)
Service ClusterIP:80
  ↓ (load-balanced)
PodIP:5000

```

### Load Balancer
- Node Port and Cluster IP created automatically
- External Load balancer route to them
- Service is exposed at static port on each worker node
- Use underlying cloud provider's load balancer feature

### External IP
- Service map to an external IP address
- Traffic ingressed into the cluster with the External IP get routed to one of the service endpoints

### External Name
- Create a DNS alias (CNAME) inside the cluster
- Provide a shortcut to an external service

# References
[[10 - Services]]
[[11 - Service Type]]