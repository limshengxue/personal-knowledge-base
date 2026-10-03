2026-10-03 18:49

Tags: [[3 - Tags/kubernetes|kubernetes]]

# Kubernetes Pod Security
- Security contexts configure process privileges and access-related settings on pods or containers.
- Pod Security Admission checks selected security standards at admission time.
- These controls are different from authenticating users or assigning RBAC permissions.

## Security Context
- Settings can include user/group identity, capability changes, privilege restrictions, and filesystem behavior.
- Pod-level and container-level settings have different scope.
- A setting is not a complete policy for network access or credentials.

## Pod Security Admission
- Namespace labels configure `enforce`, `audit`, and `warn` behavior.
- Standards include privileged, baseline, and restricted profiles.
- Enforcement can reject noncompliant pod creation; audit/warn modes provide different feedback.
- It does not simply grant shared privileges to every pod across the cluster.

Use [[Kubernetes Namespaces]] for the policy boundary, [[Kubernetes Authentication, Authorization, and Admission Control]] for API authorization, and [[Kubernetes Network Policies]] for supported traffic restrictions.

# References
[[17 - Advanced Concepts]]
[Pod Security Admission](https://kubernetes.io/docs/concepts/security/pod-security-admission/)
