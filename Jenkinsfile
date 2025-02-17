pipeline {
  agent any
  environment {
    NETLIFY_SITE_ID = 'ab051741-3056-4dc3-96ab-4de86bdf54cf'
    NETLIFY_AUTH_TOKEN = credentials('netlify-token')
  }

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

    stage('Test') {
      agent {
        docker {
          image 'node:18-alpine'
           reuseNode true
        }
      }
      steps {
        sh '''
           test -f build/index.html
           npm test
        '''
      }
    }

    stage('Deploy') {
      agent {
        docker {
          image 'node:18-alpine'
           reuseNode true
        }
      }
      steps {
        sh '''
           npm install netlify-cli 
           node_modules/.bin/netlify --version
           echo "deploying with id $NETLIFY_SITE_ID"
           node_modules/.bin/netlify deploy --dir=build --prod
        '''
      }
    }
  }

  post {
    always {
      junit 'test-results/junit.xml'
    }
  }
}
