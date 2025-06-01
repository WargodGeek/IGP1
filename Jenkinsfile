pipeline {
    agent any

    tools {
        maven 'Maven 3.8.6'
        jdk 'Java 11'
    }

    environment {
        IMAGE_NAME = 'abc-tech/java-web-app'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/YOUR-USERNAME/YOUR-REPO.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push $IMAGE_NAME'
                }
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
        }
    }
}
