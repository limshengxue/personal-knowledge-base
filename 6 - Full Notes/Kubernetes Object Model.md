2026-03-01 09:57

Tags: [[kubernetes]]

# Kubernetes Object Model
- Object model representing different persistent entities in the cluster
- They describe
	- What containerized applications we are running
	- The nodes where the applications are deploy
	- Application resource consumptions
	- Policies attached to the application (like restart, upgrade, fault tolerance)
- For each object, we declare desired state in `spec`
- Where `status` record the actual state
- The object definition manifest include `apiVersion` and `kind`
- We often use YAML to define, which convert by kubectl to JSON and sent to the API server

## Label
- Key-value pairs attached to k8s objects
- Used to organize and select a subset of objects
- We select label using label selectors
	- Equality-based
	- Set-based operator like `in`, `notin` for values and `exist` `notexist` for keys


# References
[[8 - Kubernetes Object Model]]