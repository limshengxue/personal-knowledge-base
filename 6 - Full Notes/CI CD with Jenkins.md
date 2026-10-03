2025-06-08 20:07

Tags: [[ci cd]] [[jenkins]] [[secret]] [[docker]]

# CI CD with Jenkins
## Using Jenkins in an existing project
- Create Jenkinsfile in the project directory
- Create Jobs (Pipeline) in Jenkins
	- Under Pipeline, select Pipeline script from SCM (Source code management)
![[Attachments/Pasted image 20250521105748.png]]


## Build Environment
- Install all tools/dependencies need (npm, node, ...) on Jenkins Agent
- Can lead to conflicting version when there is multiple project (project shared agent)
- Use Docker with container (need to install Docker Pipeline plug-in) can solved this problem

```groovy
pipeline {
    agent any

    stages {
        stage('w/o docker') {
            steps {
                sh 'echo "Without docker"'
            }
        }
        stage('w/ docker') {
            agent {
                docker{
                    image 'node:24-alpine'
                }
            }
            steps {
                sh 'echo "With docker"'
            }
        }
    }
}

```

### Docker vs Without Docker
- Without Docker, the controller find the agent and run the steps on the agent itself
- With Docker, the selected agent's Docker daemon pulls the image and starts the build container. The controller coordinates execution; it is not necessarily the Docker host.
- The steps then run in the container and the container will be stopped

## Workspace Sync Between Stages
- A Docker agent defined for an individual stage can get a new workspace/node. Stages sharing a top-level agent can share its workspace; separate workspaces are not a universal stage default.
	- A file created in one stage may be unavailable in a later stage if that stage uses a different workspace.
	- We can use `reuseNode` option to allow workspace sync
```groovy
stage('w/ docker') {
    agent {
        docker{
            image 'node:24-alpine'
            reuseNode true
        }
    }
    steps {
        sh '''
            echo "With docker"
            ls -la
            touch container-yes.txt
        '''

        }
    }
```


## Building and Testing for CI
```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:24-alpine'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }

        stage('Test'){

            agent{
                docker {
                    image 'node:24-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }
    }
}
```

### JUnit Test Report
- XML format file that show the status of different test cases
- Crucial for CI/CD
- Started with Java, but become popular such that most testing framework can generate JUnit Test Report
```groovy
    post{
        always{
            junit 'test-results/junit.xml'
        }
    }
```
- Configure Jenkins to generate junit report, "always" - means even when the build failed

## Using Secrets
- Dashboard > Manage Jenkins > Credentials > Add Credentials
```groovy
    environment{
        NETLIFY_SITE_ID = 'aa790363-7d3d-4507-9680-49e384bbf9ab'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }
```


## Version and Execution Boundaries
- The examples use Node 24 rather than the source's end-of-life Node 18 image; verify image and application compatibility before use.
- Docker commands execute on the selected [[6 - Full Notes/Jenkins Agent|Jenkins Agent]], not necessarily the controller.
- Store credentials in Jenkins credential facilities and scope access to the relevant stage.
- See [[Jenkins Pipeline]] for the Groovy syntax and [[Automation with Jenkins]] for controller/agent roles.

# References
[[CI with Jenkins]]
[[CD with Jenkins]]
[Docker Pipeline workspace behavior](https://www.jenkins.io/doc/book/pipeline/docker/)
[Node release lifecycle](https://nodejs.org/en/about/previous-releases)
