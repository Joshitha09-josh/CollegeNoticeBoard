pipeline {

    agent any

    stages {

        stage('Clone Code') {
            steps {
                echo 'Cloning application code from GitHub'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t balaji00002/college-notice-board:v1 .'
            }
        }

        stage('Test Application') {
            steps {
                bat 'docker images balaji00002/college-notice-board:v1'
            }
        }

        stage('Push Image') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {

                    bat 'docker login -u %DOCKER_USERNAME% -p %DOCKER_PASSWORD%'
                    bat 'docker push balaji00002/college-notice-board:v1'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl apply -f deployment.yaml'
            }
        }

        stage('Verify Kubernetes') {
            steps {
                bat 'kubectl get deployment'
                bat 'kubectl get pods'
                bat 'kubectl get service'
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check the console output.'
        }
    }
}