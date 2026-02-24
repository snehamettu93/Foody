pipeline {
    agent any

    environment {
        // Use MAVEN_OPTS if needed; ensure JAVA_HOME is set on agent if required
        MVN = 'mvn'
    }

    options {
        // Keep build logs for a week
        buildDiscarder(logRotator(numToKeepStr: '30'))
        timestamps()
    }

    stages {
        stage('Checkout') {
            steps {
                // Standard checkout
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Run a Maven build that compiles, runs tests, and packages
                sh "${MVN} -B clean package"
            }
        }

        stage('Archive') {
            steps {
                // Archive built jars from target/ (works on both Unix and Windows agents when using pipeline steps)
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('Publish Test Results') {
            steps {
                junit 'target/surefire-reports/*.xml'
            }
        }
       
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        
    }
    }

    post {
        always {
            // Always keep the workspace for debugging
            cleanWs()
        }
        success {
            echo 'Build succeeded.'
        }
        failure {
            echo 'Build failed.'
        }
    }
}
