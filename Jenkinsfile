pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                // Clone the Git repository
                git branch: 'main', url: 'https://github.com/manavanand2812/devops_lab.git'
            }
        }

        stage('Build and Deploy') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        // Log in to Docker Hub
                        sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    }
                    // Proceed with Docker commands after login
                    sh 'docker-compose down'
                    sh 'docker-compose pull'
                    sh 'docker-compose up -d'
                }
            }
        }
    }

    post {
        always {
            // Clean up unused images and containers
            sh 'docker system prune -f'
        }
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
