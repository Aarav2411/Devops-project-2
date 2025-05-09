pipeline {
    agent any

    environment {
        IMAGE_NAME = 'aarav2411/devopsproject2'
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/Aarav2411/Devops-project-2.git'
            }
        }

        stage('Build') {
            steps {
                sh './gradlew build --stacktrace'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}:${BUILD_NUMBER}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-creds', url: '']) {
                    script {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('CanaryDeploy') {
            steps {
                echo "Simulating Canary Deployment"
            }
        }

        stage('DeployToProduction') {
            steps {
                echo "Simulating Production Deployment"
            }
        }
    }
}
