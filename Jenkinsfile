pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://myapp.git'
            }
        }
        stage('Build') {
            steps {
                script {
                    docker.build('myapp:latest')
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    def app = docker.run('myapp:latest', '-p 8000:8000')
                }
            }
        }
    }
}
