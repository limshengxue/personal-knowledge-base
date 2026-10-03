2026-03-01 10:41

Tags: [[kubernetes]] [[Kubernetes Object Model]]

# Kubernetes ConfigMaps
A ConfigMap separates non-sensitive configuration from container images. [[Kubernetes Pod|Pods]] can consume its keys through environment variables, command arguments, or mounted files. Use [[Kubernetes Secrets]] for credentials.

## Creating Configuration
Literal values and file content can be imported with kubectl:

```bash
kubectl create configmap my-config --from-literal=key1=value1 --from-literal=key2=value2
kubectl create configmap permission-config --from-file=permission-reset.properties
kubectl get configmap my-config -o yaml
```

Alternatively, apply a manifest:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: customer1
data:
  TEXT1: Customer1_Company
  TEXT2: Welcomes You
  COMPANY: Customer1 Company Technology Pct. Ltd.
```

## Consuming Configuration
The following fragments belong under a Pod's `spec`; replace the example image with your application.

Import every key as an environment variable:

```yaml
containers:
  - name: myapp
    image: example/myapp:1.0
    envFrom:
      - configMapRef:
          name: customer1
```

Select a particular key:

```yaml
containers:
  - name: myapp
    image: example/myapp:1.0
    env:
      - name: COMPANY_NAME
        valueFrom:
          configMapKeyRef:
            name: customer1
            key: COMPANY
```

Mount configuration through a [[Kubernetes Volume]]; each key becomes a filename and its value becomes the file content:

```yaml
containers:
  - name: myapp
    image: example/myapp:1.0
    volumeMounts:
      - name: config-volume
        mountPath: /etc/config
        readOnly: true
volumes:
  - name: config-volume
    configMap:
      name: customer1
```

The referenced ConfigMap must exist in the Pod's namespace. Environment variables do not update automatically after a ConfigMap change; mounted files usually update eventually, except `subPath` mounts. The application must reload changed files itself.

# References
[[15 - ConfigMaps]]
[ConfigMap usage](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/)
