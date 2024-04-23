pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "nodejs-app:${env.BUILD_ID}"
    }

    stages {
        stage('Build') {
            steps {
                echo 'Installing deps...'
                script {

                    sh 'npm install'
                }
            }
        }

        stage('Docker Build') {
            steps {
                echo 'Building Docker Image...'
                script {
                    docker.build(DOCKER_IMAGE)
                }
            }
        }

        stage('Docker Run') {
            steps {
                echo 'Running Docker Container...'
                script {
                    // Останавливаем предыдущий контейнер, если он запущен
                    sh 'docker rm -f nodejs-app || true'
                    // Запуск нового контейнера
                    sh "docker run -d --name nodejs-app -p 3000:3000 ${DOCKER_IMAGE}"
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            sh 'docker rmi ${DOCKER_IMAGE} || true'
        }
    }
}
