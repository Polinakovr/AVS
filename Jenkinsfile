pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Клонируем репозиторий
                git 'https://github.com/Polinakovr/AVS/new/main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Сборка Docker-образа
                    def app = docker.build("jenkinsimage:${env.BUILD_ID}")
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Остановка и удаление старого контейнера
                    sh 'docker stop jenkins || true'
                    sh 'docker rm jenkins || true'

                    // Запуск нового контейнера
                    sh 'docker run -d --name jenkins -p 8080:8080 jenkinsimage:${env.BUILD_ID}'
                }
            }
        }
    }

    post {
        always {
            // Очистка после сборки
            cleanWs()
        }
    }
}
