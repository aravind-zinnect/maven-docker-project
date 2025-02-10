pipeline {
    agent any

    environment {
        JAVA_HOME = "C:\\Program Files\\Java\\jdk-17"
        PATH = "${JAVA_HOME}\\bin;${env.PATH}"
        DOCKER_IMAGE = 'aravindzinnect/myapp:latest'  // Change to your Docker Hub repo
    }

    stages {
        stage('Clone Repository') {
            steps {
                git credentialsId: '021f3f93-3b37-43a6-bf6c-079326e0f5b7', 
                    url: 'https://github.com/aravind-zinnect/maven-docker-project.git',
                    branch: 'main'
            }
        }

        stage('Check Environment') {
            steps {
                bat 'echo JAVA_HOME=%JAVA_HOME%'
                bat 'java -version'
                bat 'mvn -version'
                bat 'docker --version'
            }
        }

        stage('Build with Maven') {
            steps {
                bat '''
                set -e
                mvn clean package
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                bat '''
                set -e
                docker build -t %DOCKER_IMAGE% .
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', 
                    usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    
                    bat '''
                    set -e
                    docker login -u %DOCKER_USER% -p %DOCKER_PASS%
                    docker push %DOCKER_IMAGE%
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Build and Push Successful!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
