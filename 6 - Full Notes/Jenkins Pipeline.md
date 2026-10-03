2025-06-08 19:59

Tags: [[jenkins]] [[ci cd]] [[pipeline]]

# Jenkins Pipeline
- Defined using `JenkinsFile` in ruby syntax
- Broken down tasks into stage
- Example: Clone > Build > Test > Package > Deploy
- Pipeline and "Job" are similar concept in Jenkins

## Jenkins Workspace
-  For each job there will be a directory created under the `workspace` directory of Jenkins, this directory is the workspace of the job
- Is a good practice to use the Delete Workspace option before the beginning of a job, so that the directory get cleared
- Else, the job could using the workspace of its previous execution


# References
[[Jenkins Introduction]]