pipeline {
    agent any

    // Utiliza o Maven configurado no Jenkins (Global Tool Configuration)
    tools {
        maven 'Maven_3.8.6' // Substitua pelo nome definido na sua configuração do Maven
    }

    environment {
        IMAGE_NAME     = 'agro-radar-java'
        IMAGE_TAG      = 'latest'
        CONTAINER_PORT = '8080' // Porta definida no Dockerfile (EXPOSE 8080)
        HOST_PORT      = '8080' // Porta que será mapeada no host
    }

    stages {
        stage('Checkout') {
            steps {
                // Realiza o checkout do repositório Git
                git url: 'https://github.com/JeffersonAlves12/agro-radar-java', branch: 'master'
            }
        }
        stage('Build Application') {
            steps {
                script {
                    // Executa o comando Maven para compilar a aplicação e gerar o JAR (o pom.xml é lido automaticamente)
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
                    // Constrói a imagem Docker utilizando o Dockerfile presente no repositório
                    dockerImage = docker.build("${env.IMAGE_NAME}:${env.IMAGE_TAG}")
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    // Remove o container anterior (caso exista) e roda o novo container mapeando as portas
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
