pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "manu1024/ec2-app:v1"
    }

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main', url: 'https://github.com/manikanta1024-git/docker-jenkins-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh 'docker login -u $USER -p $PASS'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $DOCKER_IMAGE'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker stop ec2-container || true
                docker rm ec2-container || true
                docker run -d -p 3000:3000 --name ec2-container $DOCKER_IMAGE
                '''
            }
        }
    }
}
