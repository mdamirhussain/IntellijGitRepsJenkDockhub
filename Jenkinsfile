pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/mdamirhussain/IntellijGitRepsJenkDockhub.git'
            }
        }

        stage('Build with Maven (Docker)') {
            steps {
                sh '''
                  docker run --rm \
                    -v "$PWD":/app \
                    -v "$HOME/.m2":/root/.m2 \
                    -w /app \
                    maven:3.9.9-eclipse-temurin-17 \
                    mvn -B clean package -DskipTests
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t intellij-jenkins-app:latest .'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                  docker stop intellij-jenkins-app || true
                  docker rm intellij-jenkins-app || true
                  docker run -d -p 8081:8080 --name intellij-jenkins-app intellij-jenkins-app:latest
                '''
            }
        }
    }
}
