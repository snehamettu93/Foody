pipeline {
    agent any
    tools { maven 'Maven 3.9' }
    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Build & Test') {
            steps { bat "mvn -B clean package" }
        }
        stage('Publish Test Results') {
            steps { junit "target\\surefire-reports\\*.xml" }
        }
        stage('Archive Artifacts') {
            steps { archiveArtifacts artifacts: 'target\\*.jar', fingerprint: true }
        }
    }
    post {
        success { echo 'Build succeeded ✅' }
        failure { echo 'Build failed ❌' }
        always { cleanWs() }
    }
}