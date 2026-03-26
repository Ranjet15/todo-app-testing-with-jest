pipeline {
    agent any

    stages {

        stage('Clone Repo') {
            steps {
                git url: 'https://github.com/ansarkmemon/todo-app-testing-with-jest.git'
            }
        }

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
                sh 'npm run build'
            }
        }

        stage('Deploy to Dev') {
            steps {
                sh '''
                docker build -t todo-dev .
                docker rm -f todo-dev-container || true
                docker run -d -p 3001:3000 --name todo-dev-container todo-dev
                '''
            }
        }
    }
}