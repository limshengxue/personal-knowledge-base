2025-06-08 19:36

Tags:  [[github actions]] [[ci cd]]

# GitHub Actions Environment
- An environment represents a deployment target such as development, staging, or production.
- A [[GitHub Actions Workflow]] job can reference it to apply deployment rules and obtain scoped [[GitHub Actions Secret|secrets]].
- Environment names do not automatically create infrastructure or isolate the runner.

## Protection Rules
- Required reviewers approve jobs; they do not merely define who can initiate deployment.
- Wait timers delay a job.
- Branch/tag restrictions control which refs can deploy.
- Optional custom protection integrations can add external checks.
- Feature availability differs by GitHub plan and repository visibility. Private-repository environments are not restricted to organization accounts.

## Configure and Use
Create the environment under repository Settings, then configure its supported rules and secrets.

This is a job fragment for a workflow with a `Build` job:

```yaml
DeployDev:
  name: Deploy to Dev
  if: github.event_name == 'pull_request'
  needs: [Build]
  runs-on: ubuntu-latest
  environment:
    name: Development
    url: https://dev.example.com
  steps:
    - name: Deploy
      run: echo "Deployment step goes here"
```

- Configure required approval and restrict branches before replacing the placeholder with real deployment code.
- Do not expose deployment credentials to untrusted pull-request code.
- Deployment history records runs; an environment is not itself a provisioned runtime.

# References
[[2 - Source Materials/Videos/Youtube Videos/GitHub Actions Environment|GitHub Actions Environment]]
[Deployment rules and availability](https://docs.github.com/en/actions/reference/workflows-and-actions/deployments-and-environments)
