pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/haosss101/pythonapp.git']])
            }
        }
        stage('DockerBuild') {
            steps {
                sh 'docker build -t haosss101/jenkins2:$BUILD_NUMBER .'
            }
        }
        stage('DockerPush') {
            steps {
                withCredentials([string(credentialsId: 'DockerHubPassword', variable: 'DockerHubPassword')]) {
                    sh 'docker login -u haosss101 -p $DockerHubPassword'
                sh 'docker push haosss101/jenkins2:$BUILD_NUMBER'
                }
            }
        }
        stage('Deploy') {
            steps {
                sshagent(['ssh-agent']) {
                sh "ssh -o StrictHostKeyChecking=no andrey@192.168.188.7 'docker run -p 8085:5000 -d -t haosss101/jenkins2:$BUILD_NUMBER'"
                }
            }
        }
    }
}
