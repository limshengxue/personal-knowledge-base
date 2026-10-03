2026-03-01 10:40

Tags: [[kubernetes]]

# Kubernetes Liveness and Probe
We need to ensure the health of the container and perform restart to solve issue like deadlocks 
- We can use probe to perform the health check

For example this check the existence of /tmp/healthy every 5 seconds, first attempt delay for 15 seconds
```yaml
**livenessProbe:**  
      **exec:**  
        **command:**  
        **- cat**  
        **- /tmp/healthy**  
      **initialDelaySeconds: 15**  
      **failureThreshold: 1**  
      **periodSeconds: 5**
```

- The probing can be achieved with HTTP, TPC, RPC

## Readiness Probe
- Readiness probe is to ensure the container only show the state of 'ready' when it is ready to serve

```
   **readinessProbe:**  
          **exec:**  
            **command:**  
            **- cat**  
            **- /tmp/healthy**  
          **initialDelaySeconds: 5**   
          **periodSeconds: 5**
```


# References
[[13 - Liveness and Probe]]