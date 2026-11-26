
pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'YOUR_DOCKERHUB_USERNAME/jenkins-app'
    }

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

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat '''
                        echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
                        docker tag jenkins-app:latest %DOCKER_USER%/jenkins-app:latest
                        docker push %DOCKER_USER%/jenkins-app:latest
                        docker logout
                    '''
                }
            }
        }

        stage('Deploy Container') {
            steps {
                bat 'docker rm -f jenkins-app-container || exit /b 0'
                bat 'docker run -d --name jenkins-app-container -p 3001:3000 jenkins-app:latest'
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: Pipeline completed!'
            echo 'Application: http://localhost:3001'
        }
        failure {
            echo 'Pipeline failed. Check Console Output.'
        }
    }
}
