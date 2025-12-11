pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ademdk/tp-jenkins-maven.git'
            }
        }

        stage('Build Maven') {
            steps {
                // Build du projet et génération du .jar dans target/
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('MVN SONARQUBE') {
            steps {
                // Analyse de code avec SonarQube
                // "sonarqube-8.9" doit être le Name configuré dans Manage Jenkins -> System -> SonarQube servers
                withSonarQubeEnv('sonarqube-8.9') {
                    sh '''
                        mvn sonar:sonar \
                          -DskipTests \
                          -Dsonar.projectKey=student-management \
                          -Dsonar.projectName=student-management
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                // Construction de l'image Docker à partir du Dockerfile
                sh 'docker build -t 2001107822455/student-management:1.0 .'
            }
        }

        stage('Push Docker Image') {
            steps {
                // Login DockerHub + push de l'image
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push 2001107822455/student-management:1.0'
                }
            }
        }
    }

    post {
        success {
            // On archive le .jar généré par Maven
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
    }
}
