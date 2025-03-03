pipeline {
    agent any

    triggers {
        cron('H/3 * * * 1')  // Every 3 minutes on Mondays
    }

    tools {
        maven 'Maven 3'  // Matches your actual configured Maven tool name
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                bat './mvnw clean verify'  // Use bat instead of sh for Windows compatibility
            }
        }

        stage('Code Coverage - Jacoco') {
            steps {
                jacoco execPattern: '**/target/jacoco.exec'
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }
    }

    post {
        always {
            junit '**/target/surefire-reports/*.xml'
        }
    }
}
