2026-03-01 09:57

Tags: [[kubernetes]]

# Kubernetes Object Model
- Object model representing different persistent entities in the cluster
- They describe
	- What containerized applications we are running
	- The nodes where the applications are deploy
	- Application resource consumptions
	- Policies attached to the application (like restart, upgrade, fault tolerance)
- Many objects expose desired state in `spec` and observed state in `status`; this is not universal. For example, ConfigMaps and Secrets hold configuration in data fields.
- The object definition manifest include `apiVersion` and `kind`
- We often use YAML to define, which convert by kubectl to JSON and sent to the API server

## Label
- Key-value pairs attached to k8s objects
- Used to organize and select a subset of objects
- We select label using label selectors
	- Equality-based
	- Set-based expressions use `In`, `NotIn`, `Exists`, and `DoesNotExist` in structured selectors; command-line selector syntax uses forms such as `key in (value)` or `!key`.


## Annotations
- Annotations attach non-identifying metadata to objects.
- They can carry tool/controller configuration or descriptions.
- Unlike labels, annotations are not used by standard label selectors to select objects.
- A specific annotation's effect depends on the component reading it.

# References
[[8 - Kubernetes Object Model]]
