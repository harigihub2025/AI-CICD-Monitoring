pipeline {
    agent any

    options {
        skipDefaultCheckout(true)
    }

    stages {

        stage('Checkout') {
            steps {
                deleteDir()
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ai-cicd-nginx:latest .'
            }
        }

        stage('Test') {
            steps {
                sh '''
                    docker rm -f jenkins-test 2>/dev/null || true
                    docker run -d --name jenkins-test -p 8081:80 ai-cicd-nginx:latest
                    sleep 3
                    curl -f http://localhost:8081
                    docker rm -f jenkins-test
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker rm -f ai-cicd-nginx 2>/dev/null || true
                    docker run -d \
                      --name ai-cicd-nginx \
                      -p 8080:80 \
                      --restart always \
                      ai-cicd-nginx:latest
                '''
            }
        }

        stage('Verify') {
            steps {
                sh 'docker ps'
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline completed successfully!'
        }
        failure {
            echo 'CI/CD Pipeline failed!'
        }
    }
}