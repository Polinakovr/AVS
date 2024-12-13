pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "nginx:latest"
        DOCKER_REGISTRY = "jenkins.example.localhost"
    }

    stages {
        stage('Клонирование репозитория') {
            steps {
                script {
                    // Указываем ветку для проверки
                    checkout([$class: 'GitSCM', 
                              branches: [[name: '*/main']], // Убедитесь, что ветка указана правильно
                              userRemoteConfigs: [[url: 'https://github.com/Byleon2361/TestRepositorya']]
                    ])
                }
            }
        }

        stage('Сборка Docker-образа') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Публикация в реестр') {
            steps {
                sh 'docker tag $DOCKER_IMAGE $DOCKER_REGISTRY/$DOCKER_IMAGE'
                sh 'docker push $DOCKER_REGISTRY/$DOCKER_IMAGE'
            }
        }

        stage('Развертывание контейнера') {
            steps {
                sh 'docker stop my-app  true && docker rm my-app  true'
                sh 'docker run -d --name my-app -p 80:80 $DOCKER_REGISTRY/$DOCKER_IMAGE'
            }
        }
    }
}
