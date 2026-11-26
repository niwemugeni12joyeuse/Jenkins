pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/niwemugeni12joyeuse/Jenkins.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test'
            }
        }

        stage('Build Application') {
            steps {
                bat 'npm run build'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t jenkins-app:latest .'
            }
        }

        stage('Deploy') {
            steps {
                bat 'docker rm -f jenkins-app-container'
                bat 'docker run -d --name jenkins-app-container -p 3001:3000 jenkins-app:latest'
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: Pipeline completed successfully!'
        }

        failure {
            echo 'ERROR: Pipeline failed. Check Console Output.'
        }
    }
}