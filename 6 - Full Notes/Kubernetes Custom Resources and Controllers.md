2026-10-03 18:49

Tags: [[3 - Tags/kubernetes|kubernetes]]

# Kubernetes Custom Resources and Controllers
- Custom resources extend the API with application-specific object kinds.
- A controller supplies behavior by reconciling actual state with desired state.
- A custom resource can exist without a controller when its purpose is storing/retrieving structured data.

## Extension Options
- CustomResourceDefinition registers an additional kind through the existing API machinery.
- API aggregation forwards requests to an extension API server.
- Choose based on the required API behavior and operational ownership.

## Reconciliation
- A controller watches relevant objects and takes corrective action.
- Combining custom resources and domain-specific controllers can implement the operator pattern.
- Reconciliation should tolerate retries and partial failures.
- Registration alone does not provision an application or enforce its desired state.

## Boundaries
- Understand the [[Kubernetes Object Model]] before defining an additional resource.
- Apply [[Kubernetes Authentication, Authorization, and Admission Control]] to access and admission.
- Version the schema and plan upgrades rather than treating CRDs as disposable configuration.

# References
[[17 - Advanced Concepts]]
[Custom resources](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)
