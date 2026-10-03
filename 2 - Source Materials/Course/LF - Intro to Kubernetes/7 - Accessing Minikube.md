2025-12-13 10:48

# 7 - Accessing Minikube
- There are 3 ways to access minikube - CLI, Web UI, and API

## `kubectl`
- Allow manage local or remote cluster
- minikube comes with kubectl but only as subcommand, it is better to install a standalone

### kubectl configuration file
- client need control plane node endpoint and credentials to interact with API server
- When using with minikube, minikube by default create the config file in home directory and kubectl go and get this file.

## Minikube Dashboard
```bash
$ minikube addons enable metrics-server
$ minikube addons enable dashboard
$ minikube dashboard
```

## API
### With kubectl proxy
- We can run `**kubectl proxy**` to authenticates with an API server on control plane and makes it listen to proxy port (default 8001)

### With authentication
- Can also use bearer token, or a set of keys and certificates


# References
