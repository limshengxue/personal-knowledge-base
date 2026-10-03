2026-10-03 18:49

Tags: [[kubernetes]]

# Kubernetes Client Certificate Access
Client certificates provide an identity for [[Kubernetes Authentication, Authorization, and Admission Control|authentication]]; separate RBAC permissions determine what that identity can do. This exercise does not create a Kubernetes User object.

## Prerequisites and Safety
Use Bash, OpenSSL, and kubectl against an isolated learning cluster. The examples assume the `minikube` cluster entry and an existing `lfs158` namespace. Run certificate submission, approval, and RBAC setup with an appropriately authorized administrator context.

Keep private keys out of the vault and Git. Review the requested identity before approving a CSR; certificate approval is security-sensitive, not an automatic enrollment step. The cluster must support the chosen signer.

## Request a Certificate
The subject CN supplies username `bob`; O supplies group `learner`.

```bash
openssl genrsa -out bob.key 2048
openssl req -new -key bob.key -out bob.csr -subj "/CN=bob/O=learner"
openssl base64 -A -in bob.csr
```

Paste the resulting base64 string into `spec.request` in `signing-request.yaml`:

```yaml
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: bob-csr
spec:
  request: REPLACE_WITH_BASE64_CSR
  signerName: kubernetes.io/kube-apiserver-client
  usages:
    - client auth
```

```bash
kubectl create -f signing-request.yaml
kubectl get csr bob-csr
kubectl certificate approve bob-csr
kubectl get csr bob-csr -o jsonpath='{.status.certificate}' | openssl base64 -d -A -out bob.crt
```

Verify that the certificate was issued before configuring credentials:

```bash
openssl x509 -in bob.crt -noout -subject -dates
kubectl config set-credentials bob --client-certificate=bob.crt --client-key=bob.key
kubectl config set-context bob-context --cluster=minikube --namespace=lfs158 --user=bob
```

## Grant Namespace-Scoped Access
Apply this combined manifest as an administrator:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-reader
  namespace: lfs158
rules:
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "watch", "list"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: pod-read-access
  namespace: lfs158
subjects:
  - kind: User
    name: bob
    apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

Then test without changing your default context:

```bash
kubectl --context=bob-context auth can-i list pods
kubectl --context=bob-context get pods
```

The kubeconfig credential name is an alias; the API server derives the authenticated identity from the certificate. Plan certificate expiration, renewal, and key protection.

# References
[[2 - Source Materials/Course/LF - Intro to Kubernetes/9 - Kubernetes Authentication, Authorization, and Admission Control]]
[Certificate signing requests](https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/)
[RBAC authorization](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)
