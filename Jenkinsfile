pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout source code from GitHub
                git 'https://github.com/PratimOlym/swagger-ui.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build Docker image using the Dockerfile in the repository
                script {
                    dockerImage = docker.build("your-dockerhub-username/your-image-name:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                // Push Docker image to Docker Hub
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Deploy with Ansible') {
            steps {
                // Run Ansible playbook for deployment
                script {
                    sh 'ansible-playbook -i inventory.ini deploy.yml'
                }
            }
        }
    }

    post {
        success {
            // Notify success
            echo 'Deployment successful!'
        }
        failure {
            // Notify failure
            echo 'Deployment failed!'
        }
    }
}

