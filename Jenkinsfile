pipeline {
    agent any

    environment {
        IMAGE_NAME = "awanmh/simple-app"
        REGISTRY_CREDENTIALS = 'dockerhub-credentials'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'echo "Mulai build aplikasi (Linux)"'
                    } else {
                        bat 'echo "Mulai build aplikasi (Windows)"'
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def img = docker.build("${IMAGE_NAME}:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', REGISTRY_CREDENTIALS) {
                        def imageTag = "${IMAGE_NAME}:${env.BUILD_NUMBER}"
                        docker.image(imageTag).push()
                        docker.image(imageTag).push('latest')
                    }
                }
            }
        }
    }

    post {
        always {
            echo "Selesai build"
        }
    }
}
