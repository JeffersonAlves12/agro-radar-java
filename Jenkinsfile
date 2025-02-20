pipeline {
    agent any

    environment {
        // Define o nome e a tag da imagem que será construída
        IMAGE_NAME   = 'agro-radar-java'
        IMAGE_TAG    = 'latest'
        // Porta que a aplicação expõe no container (conforme Dockerfile)
        CONTAINER_PORT = '8080'
        // Porta do host onde o container será acessível (pode ser igual ou diferente)
        HOST_PORT      = '8080'
    }

    stages {
        stage('Checkout') {
            steps {
                // Clona o repositório do GitHub
                git 'https://github.com/JeffersonAlves12/agro-radar-java'
            }
        }
        stage('Build Application') {
            steps {
                // Constrói a aplicação usando Maven
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Constrói a imagem Docker usando o Dockerfile presente no repositório
                    dockerImage = docker.build("${env.IMAGE_NAME}:${env.IMAGE_TAG}")
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    // Remove um container com o mesmo nome se já existir (evita conflitos)
                    sh "docker rm -f agro-radar || true"
                    // Roda o container em modo detached, mapeando a porta do host para a porta do container
                    sh "docker run -d -p ${env.HOST_PORT}:${env.CONTAINER_PORT} --name agro-radar ${env.IMAGE_NAME}:${env.IMAGE_TAG}"
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
