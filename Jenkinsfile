pipeline {
    agent any

    environment {
        KUBECONFIG = '/var/lib/jenkins/.kube/config'  // Use Jenkins-accessible kubeconfig
    }

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
                    sh 'echo $Docker_Hub_Pass | docker login -u kishorethy --password-stdin'
                    sh 'docker push kishorethy/java-azure-project:latest'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f /var/lib/jenkins/workspace/java-azure-project/k8-dep-svc.yml'
            }
        }

        stage('Check Deployment Status') {
            steps {
                sh 'kubectl get all'
            }
        }
    }

    post {
        always {
            echo 'Pipeline run complete.'
        }
        success {
            echo '✅ Deployment succeeded!'
        }
        failure {
            echo '❌ Deployment failed. Check logs.'
        }
    }
}
