pipeline {
    agent any

    environment {
        IMAGE_NAME = 'yourdockerhubusername/yourapp'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/yourusername/yourrepo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}:${BUILD_NUMBER}")
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    dockerImage.inside {
                        sh 'pytest tests/'  // or any test framework you're using
                    }
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-creds', url: '']) {
                    script {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Cleanup') {
            steps {
                sh "docker rmi ${IMAGE_NAME}:${BUILD_NUMBER}"
            }
        }
    }
}
