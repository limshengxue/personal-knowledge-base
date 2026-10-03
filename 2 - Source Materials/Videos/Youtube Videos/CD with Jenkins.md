# Using CLI tool with environment variables
```
pipeline {
    agent any

    environment{
        NETLIFY_SITE_ID = 'aa790363-7d3d-4507-9680-49e384bbf9ab'
    }

    stages {
        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    npm install netlify-cli --save-dev
                    ./node_modules/.bin/netlify --version
                    echo "Deploying to production. Site ID: ${NETLIFY_SITE_ID}"
                '''
            }
        }
    }

}
```

