pipeline {
    agent any

    environment{
        NETLIFY_SITE_ID = '0ff25fc8-ff4f-4373-81e3-04fd3887e7bb'
        NETLIFY_AUTH_TOEKN = credentials('netlify-token')
    }

    stages {
        stage('Build') {

            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            } 


            steps {
                sh'''
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
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps{
                sh'''
                test -f build/index.html
                '''
            }
        }

        stage('Deploy'){
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh'''
                npm install netlify-cli
                npx netlify --version
                npx netlify status
                

                

                '''
            }
        }
    }
}
