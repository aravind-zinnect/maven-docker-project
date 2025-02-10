pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git credentialsId: '021f3f93-3b37-43a6-bf6c-079326e0f5b7', 
                    url: 'https://github.com/aravind-zinnect/maven-docker-project.git',
                    branch: 'main'
            }
        }

        stage('Build with Maven') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t myapp .'
            }
        }

        stage('Push Docker Image') {
            steps {
                bat 'docker push myapp'
            }
        }
    }

    post {
        failure {
            echo 'Build failed!'
        }
    }
}
