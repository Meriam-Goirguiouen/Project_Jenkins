pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'echo "Building the application..."'
                sh 'docker build -t my-python-app .'
            }
        }

        stage('Test') {
            steps {
                sh 'echo "Running tests..."'
                // Ajoute ici des tests si tu en as
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh 'echo "Pushing the Docker image to Docker Hub..."'
                        sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'
                        sh 'docker tag my-python-app $DOCKER_USER/my-python-app:latest'
                        sh 'docker push $DOCKER_USER/my-python-app:latest'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh 'echo "Deploying the application..."'
                        sh 'ssh user@remote-server "docker pull $DOCKER_USER/my-python-app:latest"'
                        sh 'ssh user@remote-server "docker stop my-python-app || true"'
                        sh 'ssh user@remote-server "docker rm my-python-app || true"'
                        sh 'ssh user@remote-server "docker run -d -p 5000:5000 --name my-python-app $DOCKER_USER/my-python-app:latest"'
                    }
                }
            }
        }
    }
}
