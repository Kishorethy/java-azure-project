pipeline {
    agent any

    stages {
        stage('Git Clone') {
            steps {
                git 'https://github.com/Kishorethy/java-azure-project.git'
            }
        }

        stage('Maven Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t kishorethy/java-azure-project:latest .'
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([string(credentialsId: '0c0d4578-81aa-47ed-a652-4f0a16d8235b', variable: 'Docker_Hub_Pass')]) {
                    sh 'docker login -u kishorethy -p $Docker_Hub_Pass'
                    sh 'docker push kishorethy/java-azure-project:latest'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f /var/lib/jenkins/workspace/Final/k8-dep-svc.yml'
            }
        }
    }
}
