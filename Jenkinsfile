pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/ArshiaAnjumD/college-notice-board.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat '"C:\\Users\\arshi\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" build -t arshiaanjumd/college-notice-board:1.0 .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    bat '"C:\\Users\\arshi\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" login -u %DOCKER_USER% -p %DOCKER_PASSWORD%'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat '"C:\\Users\\arshi\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\docker.exe" push arshiaanjumd/college-notice-board:1.0'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withKubeConfig([credentialsId: 'kubeconfig']) {
                    bat '"C:\\Users\\arshi\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\kubectl.exe" apply -f deployment.yaml'
                }
            }
        }

        stage('Verify Pods') {
            steps {
                withKubeConfig([credentialsId: 'kubeconfig']) {
                    bat '"C:\\Users\\arshi\\AppData\\Local\\Programs\\DockerDesktop\\resources\\bin\\kubectl.exe" get pods'
                }
            }
        }
    }
}