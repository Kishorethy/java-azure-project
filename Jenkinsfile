pipeline {
    agent any

    stages {
        stage('git clone') {
            steps {
              git 'https://github.com/SanjayKumarKS/java-azure-project.git'
            }
        }
        stage('build') {
            steps {
              sh "mvn clean package"
            }
        }
    }
}
