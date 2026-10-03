2026-03-01 10:31

Tags: [[kubernetes]] [[Kubernetes Object Model]]

# Kubernetes Service
A Service provides a stable access abstraction for changing [[Kubernetes Pod|Pod]] endpoints. A selector-based Service discovers matching Pods through labels; it does not select Deployment or ReplicaSet objects directly.

## Matching Deployment and Service Labels
A [[Kubernetes Deployment]] selector matches its Pod-template labels. A Service uses those same Pod labels; the Deployment's own metadata labels do not determine Service membership.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
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
        - name: frontend
          image: example/frontend:1.0
          ports:
            - containerPort: 5000
---
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
```

Replace the illustrative image with an application that listens on port 5000. Save both objects in `frontend.yaml`, then run `kubectl apply -f frontend.yaml`.

## Endpoints and Routing
EndpointSlice objects track backend addresses and ports. Inspect them with:

```bash
kubectl get service frontend-svc
kubectl get endpointslices -l kubernetes.io/service-name=frontend-svc
```

The older Endpoints API is deprecated from Kubernetes 1.33; prefer EndpointSlices.

### Kube-proxy
In clusters using kube-proxy, it watches Services and EndpointSlices and programs node-level forwarding rules. Implementations vary: iptables, nftables, IPVS, and replacement dataplanes are not identical. Some network plugins replace kube-proxy entirely.

Traffic policies select **endpoints**, not different Services:
- `internalTrafficPolicy: Cluster` allows cluster-wide ready endpoints; `Local` restricts internal traffic to ready endpoints on the source node.
- `externalTrafficPolicy: Local` restricts external Service traffic to node-local endpoints and preserves the client source IP.
- A node without eligible local endpoints cannot serve traffic governed by a Local policy.

## Service Types
- **ClusterIP**: default internal virtual IP.
- **NodePort**: exposes a port on nodes, usually from 30000–32767; actual reachability depends on routing and firewalls.
- **LoadBalancer**: requests an external load balancer from the infrastructure integration. NodePorts are normally allocated, but some implementations can disable them.
- **ExternalName**: provides a DNS CNAME alias, not a traffic proxy.

A multi-port Service must name each port:

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
      port: 8080
      targetPort: 80
      nodePort: 31080
    - name: https
      port: 8443
      targetPort: 443
      nodePort: 31443
```

`externalIPs` is a field, not a Service type. It assumes external routing is already arranged; it does not allocate addresses. It is deprecated in Kubernetes 1.36.

## Discovery and Debugging
Cluster DNS is the usual discovery mechanism. Service environment variables are a snapshot at Pod creation, so Services created later are not automatically added.

```bash
kubectl port-forward deployment/frontend 8080:5000
kubectl port-forward service/frontend-svc 8080:80
```

Port-forwarding is a debugging tunnel, not production exposure or a test of Service load balancing. Use [[Kubernetes Ingress]] for HTTP routing and [[Kubernetes Network Policies]] for supported network access controls.

# References
[[10 - Services]]
[[11 - Service Type]]
[Services](https://kubernetes.io/docs/concepts/services-networking/service/)
[EndpointSlices](https://kubernetes.io/docs/concepts/services-networking/endpoint-slices/)
[Internal traffic policy](https://kubernetes.io/docs/concepts/services-networking/service-traffic-policy/)
[Kubernetes 1.36 deprecations](https://kubernetes.io/blog/2026/03/30/kubernetes-v1-36-sneak-peek/)
