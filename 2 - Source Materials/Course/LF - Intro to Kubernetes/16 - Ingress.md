2026-02-28 15:24

# 16 - Ingress
- Routing rules are associated with given Services
- They are many rules because there are many Services
- Ingress decouple routing rules from application and centralize the rules management
- Configure a Layer 7 HTTP/HTTPS load balancer for Services
![[Attachments/Pasted image 20260228152616.png]]
- Users do not connect directly to Service
- They reach Ingress endpoint and the Ingress forward the request
- The common routing rules are name-based, fanout, but support other custom as well.

## Name-based Virtual Hosting
We can define a name-based virtual hosting ingress
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress

metadata:
  # Name of the Ingress resource
  name: virtual-host-ingress

  # Namespace where this ingress lives
  namespace: default

  annotations:
    # nginx-specific annotation
    # service-upstream=true means:
    # send traffic directly to Service endpoints instead of using cluster IP load balancing
    nginx.ingress.kubernetes.io/service-upstream: "true"

spec:

  # Which ingress controller should handle this
  # Must match installed ingress controller
  ingressClassName: nginx

  # Rules define how traffic is routed
  rules:

  # -----------------------------
  # Rule 1
  # -----------------------------
  - host: blue.example.com

    http:
      paths:

      - path: /

        # How path matching works depends on controller
        # ImplementationSpecific = let nginx decide
        pathType: ImplementationSpecific

        backend:
          service:

            # Service name to forward traffic to
            name: webserver-blue-svc

            port:
              number: 80


  # -----------------------------
  # Rule 2
  # -----------------------------
  - host: green.example.com

    http:
      paths:

      - path: /
        pathType: ImplementationSpecific

        backend:
          service:

            # Service for green app
            name: webserver-green-svc

            port:
              number: 80
```


## Fan-out
- Decided by path instead of name
```
**apiVersion: networking.k8s.io/v1  
kind: Ingress  
metadata:**  
  **annotations:**  
    **nginx.ingress.kubernetes.io/service-upstream: "true"**  
  **name: fan-out-ingress  
  namespace: default  
spec:  
  ingressClassName: nginx   
  rules:  
  - host: example.com  
    http:  
      paths:  
      - path: /blue  
        backend:  
          service:  
            name: webserver-blue-svc  
            port:  
              number: 80  
        pathType: ImplementationSpecific  
      - path: /green  
        backend:  
          service:  
            name: webserver-green-svc  
            port:  
              number: 80  
        pathType: ImplementationSpecific**
```

## Ingress Controllers
- Watching the Control Plane Node's API server for changes in Ingress resources and update the Load Balancer accordingly
- Also known as Service Proxy, Reverse proxy, ingress proxy
- There many types of Ingress Controllers by AWS, Nginx, etc 


# References
