pipeline {
    agent any
    stages {
        stage('build') {
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
                ls -la '''
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
        stage('E2E') {
            agent {
                docker {
                    image  'mcr.microsoft.com/playwright:v1.44.1-jammy'
                    reuseNode true
                }
            }
            
            steps {
                sh '''
                npm install serve
                node_modules/.bin/server -s build &
                sleep 20
                npm playwright test
                '''
            }
        }
    }
    post {
        always {
            junit 'jest-results/junit.xml'
        }
    }
}