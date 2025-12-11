pipeline {
    agent any

    environment {
        IMAGE_NAME = "myflaskapp"
        CONTAINER_NAME = "myflaskcontainer"
    }

    stages {

        stage('Clone Repository') {
            steps {
                checkout scm   
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image..."
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Stop Old Container if Running') {
            steps {
                script {
                    sh """
                        if [ \$(docker ps -aq -f name=${CONTAINER_NAME}) ]; then
                            echo "Stopping and removing old container..."
                            docker stop ${CONTAINER_NAME} || true
                            docker rm ${CONTAINER_NAME} || true
                        fi
                    """
                }
            }
        }

        stage('Run New Container') {
            steps {
                script {
                    echo "Running new container..."
                    sh "docker run -d -p 5000:5000 --name ${CONTAINER_NAME} ${IMAGE_NAME}"
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline executed successfully! 🎉"
        }
        failure {
            echo "Pipeline failed ❌"
        }
    }
}
