pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ademdk/tp-jenkins-maven.git'
            }
        }

        stage('Build') {
            steps {
                dir('student-management') {
                    // Linux
                    sh 'mvn clean package'

                    // Windows :
                    // bat 'mvn clean package'
                }
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: 'student-management/target/*.jar', fingerprint: true
            // ou 'student-management/target/*.war'
        }
    }
}
