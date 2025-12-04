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
                // On est déjà dans le bon dossier (pom.xml à la racine)
                // Jenkins tourne chez toi sur Linux → on garde sh
                sh 'mvn clean package'

                // Si un jour tu es sur Jenkins Windows, tu utiliseras :
                // bat 'mvn clean package'
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
