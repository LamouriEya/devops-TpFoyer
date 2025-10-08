pipeline {
    agent any

    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))  // syntaxe correcte
    }

    stages {
        stage('SonarQube Scan') {
            steps {
                // Le nom doit correspondre exactement à celui configuré dans Jenkins → "sq1"
                withSonarQubeEnv('sq1') {
                    // Commande correcte avec maven wrapper
                    sh './mvnw clean verify sonar:sonar -Dsonar.projectKey=devops-TpFoyer'
                }
            }
        }
    }
}
