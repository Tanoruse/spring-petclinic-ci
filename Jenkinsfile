pipeline {
    agent any

    // Trigger every 3 minutes on Monday (day-of-week 1)
    triggers {
        cron('H/3 * * * 1')
    }

    tools {
        maven 'maven3'  // Make sure your Jenkins Global Tool Configuration has Maven named 'maven3'
    }

    stages {
        stage('Build') {
            steps {
                // Use Maven Wrapper (mvnw) for cross-platform compatibility
                bat './mvnw clean package'
            }
        }
        stage('Test with Jacoco') {
            steps {
                // Use Maven Wrapper for tests + Jacoco report
                bat './mvnw test jacoco:report'
            }
            post {
                always {
                    // Publish the Jacoco report as an HTML report in Jenkins
                    publishHTML(target: [
                        allowMissing: false,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: 'target/site/jacoco',
                        reportFiles: 'index.html',
                        reportName: "Jacoco Coverage Report"
                    ])
                }
            }
        }
    }

    post {
        success {
            // Archive the generated artifact (e.g., jar file)
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
    }
}
