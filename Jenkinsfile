pipeline {
    agent any
    stages {
        stage('Pull Code From GitHub') {
            steps {
                git branch: 'dev', url: 'https://github.com/krishnaraj3/multibranchkube.git'
            }
        }
        stage('Build the Docker image') {
            steps {
                sh 'sudo docker build -t multibranchkube /var/lib/jenkins/workspace/multibranchkube'
                sh 'sudo docker tag multibranchkube kamalraj03/multibranchkube:latest'
                sh 'sudo docker tag multibranchkube kamalraj03/multibranchkube:${BUILD_NUMBER}'
            }
        }
        stage('Push the Docker image') {
            steps {
                sh 'sudo docker image push kamalraj03/multibranchkube:latest'
                sh 'sudo docker image push kamalraj03/multibranchkube:${BUILD_NUMBER}'
            }
        }
        stage('Deploy on Kubernetes') {
            steps {
                sh 'sudo kubectl apply -f /var/lib/jenkins/workspace/multibranchkube/pod.yaml'
                sh 'sudo kubectl rollout restart deployment loadbalancer-pod'
            }
        }
    }    
}