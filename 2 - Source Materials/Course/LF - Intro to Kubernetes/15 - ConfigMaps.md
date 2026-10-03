2026-02-28 13:43

# 15 - ConfigMaps
- Decouple configuration details from container image
- Consumed by Pods or other system components and controllers
- In the form of environment variables, set of commands, arguments, or volumes

Create config map with literal values
```yaml
**kubectl create configmap my-config \  
  --from-literal=key1=value1 \  
  --from-literal=key2=value2**
```

View the config map in yaml format
```
**kubectl get configmaps my-config -o yaml**
```

We can also create with manifest file
```
**apiVersion: v1  
kind: ConfigMap  
metadata:  
  name: customer1  
data:  
  TEXT1: Customer1_Company  
  TEXT2: Welcomes You  
  COMPANY: Customer1 Company Technology Pct. Ltd.**
```

We can also create from `.properties` file which is like 
```
**kubectl create configmap permission-config \**  
  **--from-file=<path/to/>permission-reset.properties**
```

## Using ConfigMaps inside Pods
### As Environment Variables
We can then use the values in configmaps as the environment variables in the container
```yaml
  **containers:**  
  **- name: myapp-full-container**  
    **image: myapp**  
    **envFrom:**  
    **- configMapRef:**  
      **name: full-config-map**
```

Using this manifest we set 2 environment variables from 2 different config maps
SPECIFIC_ENV_VAR1 - get SPECIFIC_DATA from from config-map-1
```yaml
  **containers:**  
  **- name: myapp-specific-container**  
    **image: myapp**  
    **env:**  
    **- name: SPECIFIC_ENV_VAR1**  
      **valueFrom:**  
        **configMapKeyRef:**  
          **name: config-map-1**  
          **key: SPECIFIC_DATA**  
    **- name: SPECIFIC_ENV_VAR2**  
      **valueFrom:**  
        **configMapKeyRef:**  
          **name: config-map-2**  
          **key: SPECIFIC_INFO**
```

### As Volume
- In this case, each key become a file in the mounted directory, and the value become the file content
```yaml
  **containers:**  
  **- name: myapp-vol-container**  
    **image: myapp**  
    **volumeMounts:**  
    **- name: config-volume**  
      **mountPath: /etc/config**  
  **volumes:**  
  **- name: config-volume**  
    **configMap:**  
      **name: vol-config-map**
```

# Secrets
- Be careful, secrets are stored as plain text in etcd unless configured to be encrypt
- Help to allow encoding of sensitive info
- We can create a secret with literal values, manifest, file
- The `get` and `describe` command will not reveal its values
- Secrets can be used as mounted volume or env variables just like config map

```yaml
**kubectl create secret generic my-password \**  
  **--from-literal=password=mysqlpassword**
 ```
 
 ```yaml
 **apiVersion: v1  
kind: Secret  
metadata:  
  name: my-password  
type: Opaque  
data:  
  password: bXlzcWxwYXNzd29yZAo=** # must be base64 encoded
 ```

```yaml
**apiVersion: v1  
kind: Secret  
metadata:  
  name: my-password  
type: Opaque  
stringData:  # with stringData map, we can provide string values
  password: mysqlpassword**
```


# References
