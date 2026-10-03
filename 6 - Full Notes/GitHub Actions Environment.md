2025-06-08 19:36

Tags:  [[github actions]] [[ci cd]]

# GitHub Actions Environment
## What is an Environment
- Logical representations of the environment
- Dev - Test - QA - Stage - Prod
- Whole environment or slice
- Can have its own secrets
- Have protection rules

### Protection Rules
- Required reviewers
	- Set the reviewer that can deploy to the environment
- Wait timer
- Allowed branches
	- Only allow deploy from certain branch
- APIs for 3rd party integration

### Deployment Logs
- For single or multiple environment
- Show the complete history of deployment

---
## Create Environment
- Only Public repo
- Private for organization account only
- Setting > Environment > New Environment
- Add Protection Rules
	- Required reviewers
	- Wait timer
- Add Deployment Branch and Secrets

## Using an Environment
```yaml
DeployDev:
	name: Deploy to Dev
	if: github.event_name == 'pull_request'
	needs: [Build]
	runs-on: ubuntu-latest

	environment:
		name: Development
		url: 'http://dev.myapp.com'

	steps :
		- name: Deploy
		  run: echo I am deploying!
```

# References
[[2 - Source Materials/Videos/Youtube Videos/GitHub Actions Environment|GitHub Actions Environment]]