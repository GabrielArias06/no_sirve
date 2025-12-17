pipeline {
    agent any

    environment {
        IMAGE_NAME = "emii160/proyecto1"
        IMAGE_TAG  = "emily-v1"
        FULL_IMAGE = "${IMAGE_NAME}:${IMAGE_TAG}"
    }

    stages {

        stage('CI #1 - Build imagen con Docker Compose') {
            steps {
                echo "Construyendo imagen Docker con docker-compose..."
                bat 'docker compose build'
            }
        }

        stage('CI #1 - Tag de la imagen') {
            steps {
                echo "Asignando tag de la imagen..."
                bat 'docker tag emii160/proyecto1:latest %FULL_IMAGE%'
            }
        }

        stage('CI #1 - Push a Docker Hub') {
            steps {
                echo "Haciendo push de la imagen..."
                bat 'docker push %FULL_IMAGE%'
            }
        }

        stage('CI #2 - Pull de imagen') {
            steps {
                echo "Descargando imagen desde Docker Hub..."
                bat 'docker pull %FULL_IMAGE%'
            }
        }

        stage('CI #2 - Verificación de imagen') {
            steps {
                echo "Verificando imagen..."
                bat 'docker images | findstr proyecto1'
            }
        }

        stage('CI #2 - Ejecución de comandos') {
            steps {
                echo "Ejecutando comandos de revisión..."
                bat '''
                docker run --rm %FULL_IMAGE% cmd /c ^
                "rpm --version && ^
                 ruby --version && ^
                 node --version && ^
                 yarn --version && ^
                 python3 --version"
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline ejecutado correctamente."
        }
        failure {
            echo "Error en el pipeline."
        }
    }
}