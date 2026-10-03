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
- Each job can run different environment (different docker containers - Linux, Windows)

## Actions
- Individual task in a workflow
- Reference an action (use existing actions)
- Authoring an action (create your own action)
	- action.yml - metadata
	- index.js - the code for the action (will execute on node js)
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
- Set permission to read and write to fix 403 errors
- Set permission header in yaml file


# References
[[CI CD with GitHub Actions]]
[[Introduction to GitHub Actions]]