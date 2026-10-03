- Free and open source automation server for *build* and *test* software
- Compile code > Run tests > Build a new version of the app > Deploy the app

# Infrastructure
- Master 
	- Control Pipeline
	- Schedule Build
- Agent
	- Execute the Build

## Types of Agent
- Agent are selected based on labels
- Agents can be 
	- Permanent Agent  - dedicated servers for running jobs
	- Cloud Agent - Dynamic agent that spin up on demand (eg. AWS, K8s, Docker Cloud)

# Types of Jenkins Jobs
- Freestyle
	- Simplest method to create a build
	- Feels like Shell Scripting
- Pipeline
	- Use JenkinsFile in ruby syntax
	- Broken down tasks into stages
	- Example: Clone > Build > Test > Package > Deploy

# DevOps
- Is NOT a standard or specification
- Is NOT a tool or particular software
- Is something you do with tools or software
- Is a cultural mindset that encourage collaboration between developers, sysadmin, tester
- Automating tasks : Build, test, and deploy software

# Jenkins Workspace
- Is a good practice to use the Delete Workspace option before the beginning of a job
- For each job there will be a directory under the workspace directory