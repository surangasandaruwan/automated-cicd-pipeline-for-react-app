pipeline {
    agent any

    stages {
        stage('Build and Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            environment {
                NODE_MODULES_CACHE = "${WORKSPACE}/.npm"
            }

            steps {
                script {
                    // Cache node_modules if available
                    if (fileExists("${NODE_MODULES_CACHE}/node_modules")) {
                        sh "cp -r ${NODE_MODULES_CACHE}/node_modules ./"
                    }
                }

                sh '''
                    echo "Listing files in the workspace:"
                    ls -la

                    echo "Node version:"
                    node --version

                    echo "NPM version:"
                    npm --version

                    echo "Installing dependencies:"
                    npm ci

                    echo "Building the project:"
                    npm run build

                    echo "Checking if build was successful and verifying output:"
                    ls -la build
                    test -f build/index.html
                '''

                script {
                    // Cache node_modules after successful build
                    sh "mkdir -p ${NODE_MODULES_CACHE} && cp -r ./node_modules ${NODE_MODULES_CACHE}/"
                }
            }
        }
    }
}
