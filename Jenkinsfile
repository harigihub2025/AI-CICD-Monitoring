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

                    docker run -d --name jenkins-test ai-cicd-nginx:latest

                    sleep 3

                    docker run --rm \
                      --network container:jenkins-test \
                      curlimages/curl \
                      -f http://localhost

                    docker rm -f jenkins-test
                '''
            }
        }

        stage('Deploy Local') {
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

        stage('Verify Local') {
            steps {
                sh 'docker ps'
            }
        }

       stage('Test EC2 SSH') {
    steps {
        withCredentials([
            sshUserPrivateKey(
                credentialsId: 'ec2-ssh',
                keyFileVariable: 'SSH_KEY',
                usernameVariable: 'SSH_USER'
            )
        ]) {
            sh '''
                chmod 600 "$SSH_KEY"

                ssh -i "$SSH_KEY" \
                    -o StrictHostKeyChecking=no \
                    "$SSH_USER@16.4.52.1" \
                    "echo EC2 SSH connection successful && hostname"
            '''
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