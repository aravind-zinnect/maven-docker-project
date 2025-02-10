pipeline {
    agent any

    environment {
        IMAGE_NAME = "myapp"
        IMAGE_TAG = "latest"
        DOCKER_REPO = "yourdockerhubusername/myapp"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/yourusername/maven-docker-project.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withDockerRegistry([credentialsId: 'docker-hub-credentials', url: '']) {
                    sh 'docker tag $IMAGE_NAME:$IMAGE_TAG $DOCKER_REPO:$IMAGE_TAG'
                    sh 'docker push $DOCKER_REPO:$IMAGE_TAG'
                }
            }
        }
    }

    post {
        success {
            echo 'Build and deployment successful!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
