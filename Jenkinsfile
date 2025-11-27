pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
        IMAGE_NAME = "mujaheed00/python-app"
    }

    stages {

        stage('Clone Repository') {
            steps {
                echo "Cloning repo..."
                git branch: 'main',
                    url: 'https://github.com/mujaheed00/Python-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh """
                    docker build -t ${IMAGE_NAME}:latest .
                """
            }
        }

        stage('Docker Login') {
            steps {
                echo "Logging into Docker Hub..."
                sh """
                    echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin
                """
            }
        }

        stage('Push Image') {
            steps {
                echo "Pushing Docker image..."
                sh """
                    docker push ${IMAGE_NAME}:latest
                """
            }
        }
    }

    post {
        always {
            script {
                echo "Pipeline finished!"
                sh "docker images"
            }
        }
    }
}

