2025-06-08 19:38

Tags: [[github actions]] [[ci cd]] [[secret]]

# GitHub Actions Secret
- Types of Secrets 
	- Organization level
		- Allow secret management at org level without duplication
		- Effectively becomes repo secret
		- Can be scoped to specific repo
		- Not available with free plan
	- Repo secrets
		- Can override org secret
	- Environment Secret
		- Override Org/Repo secret
		- Only users with environment permission can add/edit
- Secure features
	- Secret can never be viewed once added, can only be updated
	- Secret will be censored when logged

## Limitations
- Secrets cannot read in app
- Secrets are not forked by default (can enable for private repo)
- Workflow can have up to 100 secrets

## Using a secret
```yaml
jobs :
	log:
		runs-on: ubuntu-latest
	
	steps:
		- name: Log the secret
		  run: echo ${{ secrets.MY_SECRET}}
```


# References
[[GitHub Actions Secrets]]