pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'your-docker-hub-credentials-id' // Update with Docker credentials ID
        GITHUB_CREDENTIALS_ID = 'your-github-ssh-credentials-id' // Update with GitHub SSH credentials ID
        DOCKER_IMAGE = 'Panthagadi1/nginx-deploy'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git credentialsId: "${GITHUB_CREDENTIALS_ID}", url: 'git@github.com:Panthagadi1/nginx-kind-deploy.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${DOCKER_IMAGE} .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS_ID}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                sh 'docker push ${DOCKER_IMAGE}'
            }
        }

        stage('Deploy to Docker Container') {
            steps {
                sh 'docker stop nginx-container || true && docker rm nginx-container || true'
                sh 'docker run -d -p 80:80 --name nginx-container ${DOCKER_IMAGE}'
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful! 🚀'
        }
        failure {
            echo 'Deployment Failed ❌'
        }
    }
}
