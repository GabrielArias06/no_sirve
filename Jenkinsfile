pipeline {
    agent any

    environment {
        DOCKER_USER = "emily06"
        IMAGE_NAME = "proyecto1"
        IMAGE_TAG  = "ci"
        FULL_IMAGE = "${DOCKER_USER}/${IMAGE_NAME}:${IMAGE_TAG}"
    }

    stages {

        stage('CI #1 - Build imagen con Docker Compose') {
            steps {
                echo "Construyendo imagen Docker con docker-compose"
                bat 'docker compose build'
            }
        }

        stage('CI #1 - Push a Docker Hub') {
            steps {
                echo "Publicando imagen ${FULL_IMAGE}"
                bat 'docker push %FULL_IMAGE%'
            }
        }

        stage('CI #2 - Pull de imagen') {
            steps {
                echo "Descargando imagen desde Docker Hub"
                bat 'docker pull %FULL_IMAGE%'
            }
        }

        stage('CI #2 - Verificación de imagen') {
            steps {
                echo "Imagen usada: %FULL_IMAGE%"
                bat 'docker images %FULL_IMAGE%'
            }
        }

        stage('CI #2 - Ejecución de comandos') {
            steps {
                echo "Ejecutando comandos de revisión"
                bat '''
                docker run --rm %FULL_IMAGE% cmd /c ^
                "rpm --version && ^
                 ruby --version && ^
                 node --version && ^
                 yarn --version && ^
                 python --version"
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