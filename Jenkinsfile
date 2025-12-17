pipeline {
    agent any

    environment {
        IMAGE_NAME = "emii160/proyecto1"
        IMAGE_TAG  = "emily-v1"
        FULL_IMAGE = "${IMAGE_NAME}:${IMAGE_TAG}"
    }

    stages {

        // =========================
        // CI #1 - BUILD + PUSH
        // =========================
        stage('CI #1 - Build imagen con Docker Compose') {
            steps {
                echo "Construyendo imagen Docker con docker-compose"
                sh '''
                    docker compose build
                '''
            }
        }

        stage('CI #1 - Tag de la imagen') {
            steps {
                echo "Asignando tag identificable (Emily)..."
                sh '''
                    docker tag emii160/proyecto1:latest ${FULL_IMAGE}
                '''
            }
        }

        stage('CI #1 - Push a Docker Hub') {
            steps {
                echo "Haciendo push de la imagen al repositorio Docker Hub..."
                sh '''
                    docker push ${FULL_IMAGE}
                '''
            }
        }

        // =========================
        // CI #2 - PULL + EJECUCIÓN
        // =========================
        stage('CI #2 - Pull de imagen') {
            steps {
                echo "Descargando imagen desde Docker Hub..."
                sh '''
                    docker pull ${FULL_IMAGE}
                '''
            }
        }

        stage('CI #2 - Verificación de imagen') {
            steps {
                echo "Verificando nombre, tag y repositorio"
                sh '''
                    docker images | grep proyecto1
                '''
            }
        }

        stage('CI #2 - Ejecución de comandos') {
            steps {
                echo "Ejecutando comandos"
                sh '''
                    docker run --rm ${FULL_IMAGE} bash -lc "
                        echo '--- RPM VERSION ---' &&
                        rpm --version &&
                        echo '--- RUBY VERSION ---' &&
                        ruby --version &&
                        echo '--- NODE VERSION ---' &&
                        node --version &&
                        echo '--- YARN VERSION ---' &&
                        yarn --version &&
                        echo '--- PYTHON VERSION ---' &&
                        python3 --version
                    "
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