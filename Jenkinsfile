pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    stages {

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test -- --watchAll=false'
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
}