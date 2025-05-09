pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = "willbla/train-schedule"   
        DOCKER_TAG = 'latest'                          
        DOCKER_REGISTRY = 'https://hub.docker.com/repositories/aaravsk'    
        GRADLE_HOME = '/opt/gradle'                    
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from GitHub
                git 'https://github.com/Aarav2411/Devops-project-2.git'  
            }
        }

        stage('Build') {
            steps {
                script {
                    // Gradle build with --stacktrace option for better error logs
                    sh './gradlew build --stacktrace'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image and tag it with the version
                    sh 'docker build -t ${DOCKER_REGISTRY}/${DOCKER_IMAGE_NAME}:${DOCKER_TAG} .'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Login to Docker Hub (ensure you have configured Docker credentials in Jenkins)
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                    }
                    // Push the Docker image to Docker Hub
                    sh 'docker push ${DOCKER_REGISTRY}/${DOCKER_IMAGE_NAME}:${DOCKER_TAG}'
                }
            }
        }

        stage('Canary Deploy') {
            steps {
                script {
                    // Deploy to the canary environment (you can customize this to use kubectl, etc.)
                    sh 'kubectl apply -f train-schedule-kube-canary.yml'
                }
            }
        }

        stage('Deploy to Production') {
            steps {
                script {
                    // Deploy to the production environment (you can customize this to use kubectl, etc.)
                    sh 'kubectl apply -f train-schedule-kube.yml'
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Deployment Failed!'
        }
        always {
            cleanWs()
        }
    }
}
}
