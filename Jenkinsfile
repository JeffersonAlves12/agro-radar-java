pipeline {
    agent any

    environment {
        IMAGE_NAME    = 'agro-radar-java'
        IMAGE_TAG     = 'latest'
        CONTAINER_PORT= '8080'
        HOST_PORT     = '8080'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/JeffersonAlves12/agro-radar-java'
            }
        }
        stage('Build Application') {
            steps {
                // Se o agente for Windows, use bat; senão, sh.
                script {
                    if (isUnix()) {
                        sh 'mvn clean package -DskipTests'
                    } else {
                        bat 'mvn clean package -DskipTests'
                    }
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${env.IMAGE_NAME}:${env.IMAGE_TAG}")
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    if (isUnix()) {
                        sh "docker rm -f agro-radar || true"
                        sh "docker run -d -p ${env.HOST_PORT}:${env.CONTAINER_PORT} --name agro-radar ${env.IMAGE_NAME}:${env.IMAGE_TAG}"
                    } else {
                        bat "docker rm -f agro-radar || exit 0"
                        bat "docker run -d -p ${env.HOST_PORT}:${env.CONTAINER_PORT} --name agro-radar ${env.IMAGE_NAME}:${env.IMAGE_TAG}"
                    }
                }
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline concluída.'
        }
    }
}
