pipeline {
    agent { label 'my-docker-agent' }  // Replace 'my-docker-agent' with your node label
    stages {
        stage('Checkout Code') {
            steps {
                git 'git@github.com:Panthagadi1/nginx-kind-deploy.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-nginx-app .'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: 'docker-hub-credentials', url: '']) {
                    sh 'docker push my-nginx-app:latest'
                }
            }
        }
        stage('Deploy to Docker Container') {
            steps {
                sh 'docker run -d -p 80:80 my-nginx-app'
            }
        }
    }
}
