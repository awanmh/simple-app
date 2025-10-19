pipeline {
    agent any
    environment {
        IMAGE_NAME = "awanmh/simple-app"
        REGISTRY_CREDENTIALS = 'dockerhub-credentials' // ID credentials di Jenkins
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
        }
    }
    stage('Build') {
        steps {
            sh 'echo "Mulai build aplikasi"'
        }
    }
    stage('Build Docker Image') {
        steps {
            script {
// build image (menggunakan Docker daemon host yang tersedia)
                def img = docker.build("${IMAGE_NAME}:${env.BUILD_NUMBER}")
            }
        }
    }
    stage('Push Docker Image') {
        steps {
            script {
                docker.withRegistry('https://index.docker.io/v1/', "$
                {REGISTRY_CREDENTIALS}") {
                def imageTag = "${IMAGE_NAME}:${env.BUILD_NUMBER}"
                docker.image(imageTag).push()
// optional: push latest tag
                docker.image(imageTag).push('latest')
                    }
                }
            }
        }
    }
    post {
        always {
            4
            echo "Selesai build"
        }
    }
}