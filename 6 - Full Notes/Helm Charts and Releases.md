2026-10-03 18:49

Tags: [[3 - Tags/kubernetes|kubernetes]]

# Helm Charts and Releases
- Helm packages related Kubernetes manifests into charts and manages installations as releases.
- A chart is a reusable package; a release is an installed instance with selected values.

## Chart Contents
- Chart metadata describes the package.
- Templates generate Kubernetes resource manifests.
- Values supply configurable inputs.
- Dependencies can compose additional charts.

## Development Workflow
- Inspect chart templates and defaults before installing them.
- Render with `helm template` and inspect the generated resources.
- `helm lint` checks chart structure.
- An application-project example is `helm upgrade --install demo ./demo-chart`; the chart directory must exist.

## Operational Boundaries
- Packaging does not guarantee application correctness, secure defaults, or safe database migration.
- Review rendered [[Kubernetes Deployment]], [[Kubernetes Service]], and storage resources.
- Keep credentials out of committed values files.
- A Helm rollback does not automatically undo every external or persistent-data change.

# References
[[17 - Advanced Concepts]]
[Helm charts](https://helm.sh/docs/topics/charts/)
