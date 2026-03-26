pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    triggers {
        pollSCM('H/5 * * * *')
    }

    environment {
        IMAGE_NAME = 'react-app'
        DEV_CONTAINER = 'react-app-dev'
        DEV_PORT = '3001'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/Ranjet15/todo-app-testing-with-jest.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test -- --watchAll=false --forceExit'
            }
        }

        stage('Build App') {
            steps {
                sh '''
                    export NODE_OPTIONS=--openssl-legacy-provider
                    npm run build
                '''
            }
        }

        stage('Deploy to Dev') {
            steps {
                sh '''
                    docker stop react-app-dev || true
                    docker rm react-app-dev || true
                    docker build --no-cache -t react-app:dev .
                    docker run -d --name react-app-dev -p 3001:80 react-app:dev
                '''
            }
        }
    }

    post {
        success {
            echo '✅ CI passed! Dev deployed at http://localhost:3001'
        }
        failure {
            echo '❌ Pipeline failed. Dev deployment skipped.'
        }
    }
}