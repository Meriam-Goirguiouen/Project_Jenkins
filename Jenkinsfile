pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials')
    }

    stages {
        stage('Build') {
            steps {
                echo "Building the application..."
                sh 'docker build -t my-python-app .'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                // Remplace cette ligne par les vraies commandes de tests, exemple :
                // sh 'pytest tests/'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo "Pushing the Docker image to Docker Hub..."
                sh '''
                    docker login -u ${DOCKER_HUB_CREDENTIALS_USR} -p ${DOCKER_HUB_CREDENTIALS_PSW}
                    docker tag my-python-app ${DOCKER_HUB_CREDENTIALS_USR}/my-python-app:latest
                    docker push ${DOCKER_HUB_CREDENTIALS_USR}/my-python-app:latest
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying the application..."
                sh '''
                    ssh user@remote-server "docker pull ${DOCKER_HUB_CREDENTIALS_USR}/my-python-app:latest"
                    ssh user@remote-server "docker stop my-python-app || true"
                    ssh user@remote-server "docker rm my-python-app || true"
                    ssh user@remote-server "docker run -d -p 5000:5000 --name my-python-app ${DOCKER_HUB_CREDENTIALS_USR}/my-python-app:latest"
                '''
            }
        }
    }
}