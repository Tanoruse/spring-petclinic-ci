pipeline {
    agent any

    triggers {
        cron('H/3 * * * 1')  // Every 3 minutes on Mondays
    }

    tools {
        maven 'Maven 3'  // Make sure your Jenkins has a Maven tool installed called 'Maven 3'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                sh './mvnw clean verify'
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
