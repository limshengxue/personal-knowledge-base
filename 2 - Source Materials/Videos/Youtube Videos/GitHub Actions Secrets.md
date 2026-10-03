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

# Limitations
- Secrets cannot read in app
- Secrets are not forked by default (can enable for private repo)
- Workflow can have up to 100 secrets