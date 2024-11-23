pipeline {
    agent { label 'slavenode' } // Ensure this label matches your agent setup

    environment {
        REPO_URL = 'https://github.com/SatyaSri-154/firstpipeline.git' // Replace with your Git repo
        IMAGE_NAME = 'lakshmi-image'                              // Docker image name
        CONTAINER_NAME = 'lakshmi-container'                      // Docker container name
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out code from Git repository...'
                git branch: 'main', url: "${REPO_URL}"
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh """
                docker build -t ${IMAGE_NAME} .
                """
            }
        }

        stage('Run Docker Container') {
            steps {
                echo 'Running Docker container...'
                sh """
                # Stop and remove any existing container
                docker stop ${CONTAINER_NAME} || true
                docker rm ${CONTAINER_NAME} || true
                
                # Run the container
                docker run -itd --name ${CONTAINER_NAME} -p 8082:80 ${IMAGE_NAME}
                """
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed.'
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed. Cleaning up resources...'
            sh """
            docker stop ${CONTAINER_NAME} || true
            docker rm ${CONTAINER_NAME} || true
            """
        }
    }
}
