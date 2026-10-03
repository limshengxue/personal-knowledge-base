2025-06-08 19:11

# GitHub Actions
- Fully integrated with GitHub
- Respond to GitHub event (push, merge, new issue ...)
- Community-powered workflows (obtain automation from the community)
- Any platform, any language, any cloud
- Can run on different containers (linux, Windows, macOS)
- Matrix execution (run the workflow with multiple variables)
- Logging
- Built-in secret store
- Easy to write and share (YAML)

# Events
- GitHub triggered events: push, pull request, ...
- Scheduled event
- Manually triggered: workflow dispatch (external system)
	- Can be useful for development

# Workflows
- Like pipelines
- Codify useful, customized processes
- yaml syntax
- Stored in .github/workflows
- Logs streaming and artifacts
- Secret store
Glue together actions
- Listen for event
- Run pre-existing actions/or shell scripts
Workflow can have multiple jobs, each jobs can then consists multiple Actions

# Actions
- Individual task in a workflow
- Reference an action (use existing actions)
- Authoring an action (create your own action)
	- action.yml - metadata
	- index.js - the code for the action (will execute on node js)
- `actions/checkout` - download the content of the repo to the runner machine


## Governance
- Repo Setting > Action
- Can set the Actions permissions to specify which set of Actions can be utilized

## Troubleshooting Tools
- Workflow editor/ VSCode extension (linting to show invalid arguments, etc)
- Action-debugging
	- Need to setup secrets
	- ACTIONS_STEP_DEBUG or ACTIONS_RUNNER_DEBUG


Set permission to read and write to fix 403 errors
Set permission header in yaml file



# References
