2025-12-14 11:24

# 11 - Service Type
- We specify service type using `type` key in the manifest yaml

## ClusterIP
- Default service type
- Service receive a Virtual IP address, known as its Cluster IP
- Only accessible within the cluster

## NodePort
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


## Load Balancer
- Node Port and Cluster IP created automatically
- External Load balancer route to them
- Service is exposed at static port on each worker node
- Use underlying cloud provider's load balancer feature

## External IP
- Service map to an external IP address
- Traffic ingressed into the cluster with the External IP get routed to one of the service endpoints

## External Name
- Create a DNS alias (CNAME) inside the cluster
- Provide a shortcut to an external service




# References
