2025-06-08 19:20

Tags:  [[github actions]] [[ci cd]] [[pipeline]] [[docker]]

# GitHub Actions Workflow
## Workflows
- Like pipelines (glue together actions)
- Codify useful, customized processes
- Defined using`yaml` syntax
- Stored in `.github/workflows` directory
- Secret store
- Glue together actions
- Listen for event
- Run pre-existing actions/or shell scripts

### Job
- Actions within a Workflow are separated into jobs
- A Workflow can have multiple jobs
- Jobs can target different runner environments, such as Linux, Windows, or macOS. A job-level container is a separate option that requires a Linux runner.

## Actions
- Individual task in a workflow
- Reference an action (use existing actions)
- Authoring an action (create your own action)
	- action.yml - metadata
	- JavaScript actions run a declared script; composite actions and Docker actions use different implementations.
- An important action is`actions/checkout` - download the content of the repo to the runner machine


### Project Governance
- Access from Repo Setting > Action
- Control the set of Actions that can be utilised


## Events
- Triggers for a workflow
- GitHub triggered events: push, pull request, ...
- Scheduled event
- Manually triggered: workflow dispatch (external system)
	- Can be useful for development

## Troubleshooting Tips for Building Workflow 
- Workflow editor/ VSCode extension (linting to show invalid arguments, etc)
- Action-debugging
	- Need to setup secrets
	- ACTIONS_STEP_DEBUG or ACTIONS_RUNNER_DEBUG
- For a 403, identify the denied resource and operation before changing permissions.
- Grant only the required `GITHUB_TOKEN` permissions at workflow or job scope; do not default to broad read/write authority.
- Some operations require a different credential or repository policy change, not a wider token permission.


## CI and Deployment
- GitHub Actions combines event-triggered automation, community actions, matrices, logging, and scoped secrets.
- [[6 - Full Notes/GitHub Actions Runner|GitHub Actions Runner]] supplies the execution environment.
- [[GitHub Actions Secret]] manages sensitive inputs.
- [[6 - Full Notes/GitHub Actions Environment|GitHub Actions Environment]] adds supported deployment gates.
- CI commonly checks out code, builds it, and runs tests or linting. The selected checks must fit the repository.
- CD adds intentional deployment credentials, approvals, and a destination; it is not just another build step.

### Source CI Example
![[Attachments/Pasted image 20250520110905.png]]

### Source Deployment Example
![[Attachments/Pasted image 20250520111413.png]]

These images preserve the original course examples; verify action versions and permissions before copying them.

# References
[[Introduction to GitHub Actions]]
[[GitHub Actions CI CD]]
[Least privilege token permissions](https://docs.github.com/en/actions/tutorials/authenticate-with-github_token)
