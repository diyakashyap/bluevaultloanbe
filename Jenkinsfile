pipeline {
    agent any

    environment {
        // Updated environment variables for the backend service
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        DOCKER_IMAGE = 'diya0311/bluevaultloanbe'
        KUBERNETES_DEPLOYMENT = 'loan-form-be'
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Point to the backend repository
                git branch: 'master', url: 'https://github.com/diyakashyap/bluevaultloanbe.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Generate a dynamic tag based on build number
                    env.TAG = "${env.BUILD_NUMBER}"
                    
                    // Build the Docker image with the dynamic tag
                    // Your Dockerfile for the backend should install Python and its dependencies
                    sh "docker build -t $DOCKER_IMAGE:$TAG ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push the new Docker image with the dynamic tag
                    withDockerRegistry([credentialsId: 'dockerhub', url: 'https://index.docker.io/v1/']) {
                        sh "docker push $DOCKER_IMAGE:$TAG"
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Use kubectl apply to deploy the new version
                    // This command will require you to have deployment.yaml and svc.yaml files
                    // and a sed command to replace the image tag.
                    sh "kubectl set image deployment/$KUBERNETES_DEPLOYMENT loan-backend=$DOCKER_IMAGE:$TAG"
                }
            }
        }
    }
}
