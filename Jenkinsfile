pipeline {
    agent any

    environment{
        NETLIFY_SITE_ID = '244c1537-8c64-4cf8-b9df-b90cd3384eed'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

    stages {
        stage('Build') {
            agent{
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
            agent{
                docker {
                    image 'node:18-alpine'
                    reuseNode true 
                }
            }
            steps {
               sh '''
                 
                 npm test
               ''' 
            }
        }


        stage('Deploy') {
            agent{
                docker {
                    image 'node:18-alpine'
                    reuseNode true 
                }
            }
            steps {
                sh '''
                  npm install netlify-cli
                  node_modules/.bin/netlify --version
                  echo "deploying to :$NETLIFY_SITE_ID"
                  node_modules/.bin/netlify deploy --dir=build --prod
                '''
            }
        }
    }
}
