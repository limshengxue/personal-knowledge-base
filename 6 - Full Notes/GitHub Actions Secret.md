2025-06-08 19:38

Tags: [[github actions]] [[ci cd]] [[secret]]

# GitHub Actions Secret
- Secrets hold sensitive values made available to authorized workflow jobs.
- Organization secrets can be scoped to selected repositories. GitHub Free organizations cannot use organization secrets in private repositories; this is not a blanket ban for public repositories.
- Repository secrets override organization secrets with the same name; environment secrets take precedence over both.
- Environment secrets become available only to jobs referencing that environment, after required approval.

## Limits and Visibility
- Limits are scope-specific: up to 1,000 organization secrets, 100 repository secrets, and 100 secrets per environment.
- A workflow can access repository and environment secrets plus up to 100 assigned organization secrets; “100 secrets total” is not the general rule.
- Saved values cannot be retrieved through the management UI, but a job/application receiving them can read them.
- Forked pull-request workflows normally do not receive repository secrets; exceptions depend on event and repository configuration.
- Log redaction is not a complete leakage safeguard, especially for transformed or structured values. Do not echo secrets for testing.

## Supply a Secret to a Job
This fragment assumes an existing secret and an application expecting `API_TOKEN`:

```yaml
jobs:
  verify:
    runs-on: ubuntu-latest
    steps:
      - name: Validate secret configuration
        env:
          API_TOKEN: ${{ secrets.API_TOKEN }}
        run: test -n "$API_TOKEN"
```

The shell checks presence without printing the value. Pass secrets only to trusted code and actions.

See [[GitHub Actions Workflow]] for job structure and [[6 - Full Notes/GitHub Actions Environment|GitHub Actions Environment]] for approval boundaries.

# References
[[GitHub Actions Secrets]]
[Secret limits](https://docs.github.com/en/actions/reference/security/secrets)
[Using secrets](https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-secrets)
