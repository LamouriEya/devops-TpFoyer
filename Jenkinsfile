pipeline {
    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }

    stages {
        stage('Checkout') {
            steps {
                // Nettoyage du workspace
                deleteDir()
                checkout scm
            }
        }

        stage('SonarQube Scan') {
            steps {
                withSonarQubeEnv('sq1') {
                    // ✅ Donner les droits d’exécution à mvnw
                    sh 'chmod +x mvnw'
                    // ✅ Exécution du build et analyse
                    sh './mvnw clean verify sonar:sonar -Dsonar.projectKey=devops-TpFoyer'
                }
            }
        }
    }
}
