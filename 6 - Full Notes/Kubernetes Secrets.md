2026-10-03 18:49

Tags: [[kubernetes]] [[Kubernetes Object Model]]

# Kubernetes Secrets
A Secret holds sensitive values separately from ordinary [[Kubernetes ConfigMaps]]. Pods can consume Secrets through environment variables or mounted files, but this does not automatically make credentials safe.

## Encoding Is Not Encryption
- `data` contains base64-encoded values; base64 is reversible, not encryption.
- `stringData` accepts plaintext input that the API converts to `data`.
- Secrets are stored unencrypted in etcd by default unless encryption at rest is configured.
- Authorized callers can retrieve values with `kubectl get secret <name> -o yaml`; hiding them in a default listing is not access control.

This learning manifest contains a placeholder, not a usable password:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: application-credentials
type: Opaque
stringData:
  password: REPLACE_WITH_SECURE_VALUE
```

## Safe Consumption
Reference a specific key using `env[].valueFrom.secretKeyRef`, or mount a `volumes[].secret` volume into the container. Keep the Secret and Pod in the same namespace.

Restrict read access through [[Kubernetes Authentication, Authorization, and Admission Control|RBAC]], configure encryption at rest, avoid committing plaintext manifests, and limit which containers receive credentials. Rotate credentials deliberately: environment variables need a Pod restart, while file updates still require application reload support.

# References
[[15 - ConfigMaps]]
[Secrets and security considerations](https://kubernetes.io/docs/concepts/configuration/secret/)
