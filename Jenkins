pipeline {
    agent any
    tools {
        maven 'Apache Maven 3.5.2'
    }
    stages{
        stage('Checkout') {
            steps {
                git 'https://github.com/vyjorg/LPDM-Config.git'
            }
        }
        stage('Tests') {
            steps {
                sh 'mvn clean test'
            }
            post {
                always {
                    junit 'target/surefire-reports/**/*.xml'
                }
                failure {
                    error 'The tests failed'
                }
            }
        }
        stage('Push to DockerHub') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker stop LPDM-ConfigMS || true && docker rm LPDM-ConfigMS || true'
                sh 'docker pull vyjorg/lpdm-config:latest'
                sh 'docker run -d --name LPDM-ConfigMS -p 28088:28088 --restart always --memory-swappiness=0  vyjorg/lpdm-config:latest'
            }
        }
    }
}