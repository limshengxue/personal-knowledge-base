2026-03-01 10:11

Tags: [[kubernetes]]

# Kubernetes Authentication, Authorization, and Admission Control
API requests pass through identity checking, permission checking, and, where applicable, object admission policy. These are separate controls, not interchangeable protections.

## Authentication: Who Is Calling?
Normal user identities come from external credentials, such as client certificates, OIDC tokens, or cloud-provider integrations. Kubernetes has no persistent `User` API object: the authenticator supplies a username and groups.

A kubeconfig `users:` entry is a client-side credential alias, not a cluster user account. See [[Kubernetes Client Certificate Access]] for a certificate-based learning example.

ServiceAccounts are namespace-scoped Kubernetes objects for workload identities. Bound tokens can be projected into Pods; disable automatic token mounting where API access is unnecessary. Anonymous access and impersonation depend on cluster configuration and permissions. Static username/password-file authentication is obsolete and should not be presented as a current method.

## Authorization: Is This Request Allowed?
Authorization evaluates attributes such as identity, verb, resource, namespace, and object name. Available mechanisms include Node authorization, RBAC, ABAC, and webhooks.

RBAC RoleBindings match identity strings: a `kind: User` subject does not refer to a stored User object. Roles and RoleBindings apply within [[Kubernetes Namespaces]]; ClusterRoles and ClusterRoleBindings support cluster-wide scopes.

## Admission: Does the Object Meet Policy?
After authentication and authorization, admission may modify or reject applicable requests before persistence. Mutating admission runs before validating admission.

Admission does not apply to every request: read operations such as get, list, and watch bypass it. Examples include [[Kubernetes Resource Quotas and Limits|ResourceQuota and LimitRanger]], [[Kubernetes Pod Security|PodSecurity]], and DefaultStorageClass.

Administrators configure built-in plugins and webhooks. The order of names in `--enable-admission-plugins` does not define their execution order.

# References
[[9 - Kubernetes Authentication, Authorization, and Admission Control]]
[Authentication](https://kubernetes.io/docs/reference/access-authn-authz/authentication/)
[Admission controllers](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/)
