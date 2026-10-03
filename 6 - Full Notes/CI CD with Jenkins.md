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

```yaml
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
                    image 'node:18-alpine'
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
- With Docker, the controller download the image and start the container
- The steps then run in the container and the container will be stopped

## Workspace Sync Between Stages
- By default, stages use different workspaces
	- When we create file in stage 1, the file cannot be accessed by stage 2
	- We can use `reuseNode` option to allow workspace sync
```yml
stage('w/ docker') {
    agent {
        docker{
            image 'node:18-alpine'
            reuseNode true # set this to allow workspace sync
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
```yaml
pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
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
                    image 'node:18-alpine'
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
```
    post{
        always{
            junit 'test-results/junit.xml'
        }
    }
```
- Configure Jenkins to generate junit report, "always" - means even when the build failed

## Using Secrets
- Dashboard > Manage Jenkins > Credentials > Add Credentials
``` using the credentials
    environment{
        NETLIFY_SITE_ID = 'aa790363-7d3d-4507-9680-49e384bbf9ab'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }
```


# References
[[CI with Jenkins]]
[[CD with Jenkins]]
