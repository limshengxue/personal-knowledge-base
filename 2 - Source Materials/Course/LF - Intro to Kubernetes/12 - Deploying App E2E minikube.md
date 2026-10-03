2026-02-28 09:06

# 12 - Deploying App E2E minikube
## Using Dashboard
- Using the '+' button we can easily deploy a service in minikube

A deployment .yaml
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webserver
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx  ## points towards pod label 
  template:
    metadata:
      labels:
        app: nginx  ## pod label (very important)
    spec:
      containers:
      - name: nginx
        image: nginx:alpine
        ports:
        - containerPort: 80
```

## Using Label and Selectors
We use `-L` to add columns to our result, with attach label keys and values
For example, `**kubectl get pods -L k8s-app,label2**` add k8s-app and label2

To use selector, we use `-l`
`**kubectl get pods -l k8s-app=web-dash**`


## Exposing a Deployment
```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-service
  labels:
    app: nginx
spec:
  type: NodePort 
  ports:
  - port: 80 
    protocol: TCP
  selector:
    app: nginx ## points towards pod
```

View the relationship of Pod IP and Service endpoint
```yaml
$ kubectl get po -l app=nginx -o wide  
$ kubectl get ep web-service
```

For Minikube clusters on the Docker driver, the NodePort cannot be accessed from the host workstation due to limitations of the Docker networking model. In those scenarios, another application access option is via the **minikube tunnel**. This option allows the Service ClusterIP to be directly exposed on the host as an External IP. First expose the **webserver** application through a LoadBalancer Service, and then enable the tunnel:

**$ kubectl expose deployment webserver --name=web-lb --type=LoadBalancer --port=8080**

**$ minikube tunnel**

**$ kubectl get services**

# References
