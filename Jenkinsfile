pipeline {
  agent any

  tools {
    maven 'Maven3'     // nom exact configuré dans "Manage Jenkins" -> "Global Tool Configuration"
    jdk 'JDK17'        // idem
  }

  environment {
    // 🔹 Ton token SonarQube (Secret Text dans Jenkins → Credentials → ID = 'jenkins-sonar')
    SONAR_TOKEN = credentials('jenkins-sonar')

    // 🔹 Adresse de ton SonarQube (remplace par la tienne si différente)
    SONAR_HOST = 'http://192.168.33.10:9000'
  }

  stages {

    stage('Checkout') {
      steps {
        // clone ton dépôt GitHub (le tien, pas celui de ton amie)
        git branch: 'main', url: 'https://github.com/LamouriEya/devops-TpFoyer.git'
      }
    }

    stage('Prepare') {
      steps {
        // Assure-toi que mvnw est exécutable (important sur Linux)
        sh 'chmod +x mvnw || true'
      }
    }

    stage('Clean') {
      steps {
        // Nettoyage complet du projet
        sh './mvnw clean -B'
      }
    }

    stage('Compile') {
      steps {
        // Compilation du code sans exécuter les tests
        sh './mvnw -B -DskipTests=true compile'
      }
    }

    stage('SonarQube Analysis') {
      steps {
        // Analyse de la qualité du code via SonarQube
        withSonarQubeEnv('sq1') {
          sh """
            ./mvnw -B sonar:sonar \
              -Dsonar.projectKey=devops-TpFoyer \
              -Dsonar.host.url=${SONAR_HOST} \
              -Dsonar.login=${SONAR_TOKEN}
          """
        }
      }
    }

    stage('Package') {
      steps {
        // Génération du fichier JAR
        sh './mvnw -B -DskipTests=true package'
      }
    }

    stage('Archive Artifact') {
      steps {
        // Sauvegarde du JAR dans Jenkins (build artifacts)
        archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
      }
    }
  }

  post {
    success {
      echo "✅ Build et analyse SonarQube terminés avec succès !"
    }
    failure {
      echo "❌ Le build a échoué. Vérifie les logs dans la console Jenkins."
    }
  }
}
