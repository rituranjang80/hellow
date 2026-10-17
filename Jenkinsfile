pipeline {
    agent any

    tools {
        // Must match the name in Manage Jenkins -> Global Tool Configuration
        jdk "jdk"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify Java') {
            steps {
                bat 'java -version'
            }
        }

        stage('Build and Test') {
            steps {
                // Use the Maven wrapper (must exist in the repo)
                bat 'mvnw.cmd -B clean verify'
            }
        }

        stage('Archive JAR') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                // allowEmptyResults: true prevents failure when no tests exist
                junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'
            }
        }
    }

    post {
        failure {
            echo 'Build failed. Check Console Output.'
        }
        success {
            echo 'Build succeeded!'
        }
    }
}