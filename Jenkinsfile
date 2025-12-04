pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Récupère le code depuis ton dépôt GitHub
                git branch: 'main', url: 'https://github.com/ademdk/tp-jenkins-maven.git'
            }
        }

      stage('Build') {
    steps {
        // On construit mais on ignore les tests
        sh 'mvn clean package -DskipTests'
    }
}

    }

    post {
        success {
            // Le JAR sera dans target/ directement
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            // Pour une appli .war, tu mettrais : 'target/*.war'
        }
    }
}
