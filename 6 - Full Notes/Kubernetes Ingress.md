2026-03-01 10:42

Tags: [[kubernetes]] [[Kubernetes Object Model]]

# Kubernetes Ingress
An Ingress describes HTTP/HTTPS routing to [[Kubernetes Service|Services]], commonly by hostname or URL path. It requires a compatible controller; creating an Ingress object alone does not create a working entry point.

![[Attachments/Pasted image 20260228152616.png]]

## Controller Lifecycle
The community **ingress-nginx** project retired on March 24, 2026. Existing installations may continue running but receive no future security fixes or releases. Do not choose it for a new deployment; evaluate a maintained controller or Gateway API implementation.

This retirement does not remove the Kubernetes Ingress API and does not apply to every product containing “NGINX” in its name. The examples below preserve the course's legacy ingress-nginx configuration, not a current installation recommendation.

## Name-based Virtual Hosting
The controller class and backend Services must exist in the same namespace as shown.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: virtual-host-ingress
  namespace: default
  annotations:
    nginx.ingress.kubernetes.io/service-upstream: "true"
spec:
  ingressClassName: nginx
  rules:
    - host: blue.example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: webserver-blue-svc
                port:
                  number: 80
    - host: green.example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: webserver-green-svc
                port:
                  number: 80
```

For ingress-nginx, `service-upstream: "true"` selects the Service ClusterIP as the upstream instead of routing directly to individual Pod endpoints. It is controller-specific, not a general Ingress feature.

## Path-based Fan-out
One hostname can route different prefixes to different Services:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: fan-out-ingress
  namespace: default
spec:
  ingressClassName: nginx
  rules:
    - host: example.com
      http:
        paths:
          - path: /blue
            pathType: Prefix
            backend:
              service:
                name: webserver-blue-svc
                port:
                  number: 80
          - path: /green
            pathType: Prefix
            backend:
              service:
                name: webserver-green-svc
                port:
                  number: 80
```

These rules do not strip the URL prefix automatically. TLS, DNS, address exposure, and any rewrite behavior require additional configuration. [[Kubernetes Network Policies]] control supported Pod traffic boundaries; they are not a replacement for Ingress routing.

# References
[[16 - Ingress]]
[Ingress API](https://kubernetes.io/docs/concepts/services-networking/ingress/)
[ingress-nginx retirement](https://kubernetes.io/blog/2026/04/22/kubernetes-v1-36-release/)
[Legacy service-upstream annotation](https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/annotations/#service-upstream)
