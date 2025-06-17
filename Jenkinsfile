pipeline {
    agent any

    environment {
        ECR_REGISTRY = '010426082127.dkr.ecr.us-east-1.amazonaws.com'
        ECR_REPO = 'app-deploy' // Replace with your ECR repository name
        IMAGE_TAG = 'latest'
    }

    stages {
         stage('Clone Repo') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/Sujith2810/sample-js.git',
                        credentialsId: 'GitHub'
                    ]]
                ])
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker build -t app-deploy .
                }
            }
        }

        stage('Login to ECR') {
            steps {
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 010426082127.dkr.ecr.us-east-1.amazonaws.com'
            }
        }

        stage('Tag & Push to ECR') {
            steps {
                script {
                    sh "docker tag app-deploy:latest 010426082127.dkr.ecr.us-east-1.amazonaws.com/app-deploy:latest"
                    sh "docker push 010426082127.dkr.ecr.us-east-1.amazonaws.com/app-deploy:latest"
                }
            }
        }
    }
}
