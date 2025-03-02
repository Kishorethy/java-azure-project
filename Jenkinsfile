pipeline {
    agent any

    stages {
        stage('git clone') {
            steps {
              git 'https://github.com/SanjayKumar-KS/java-azure-project.git'
            }
        }
        stage('build') {
            steps {
              sh "mvn clean package"
            }
        }
        stage('Build Docker OWN image') {
            steps {
                sh "sudo docker build -t kuduvasanjaykumar/javasantab51:latest ."

            }
        }
        stage('docker push ') {
     steps {
     withCredentials([string(credentialsId: 'Docker_Hub', variable: 'Docker_Hub_Pass')]) {
    // some block
        sh "sudo docker login -u kuduvasanjaykumar -p $Docker_Hub_Pass"
        }
        sh "sudo docker push kuduvasanjaykumar/javasantab51:latest"
        }
        }  
            stage('Deploy webAPP in kube Env') {
    steps {
  
     sh "kubectl apply -f /var/lib/jenkins/workspace/Final/k8-dep-svc.yml"   
  
}
}   
    }
}
