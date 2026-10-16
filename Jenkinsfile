
pipeline {
    agent any

    options {
        timestamps()
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Check Java') {
            steps {
                bat 'java -version'
            }
        }

        stage('Build and Test') {
            steps {
                bat 'mvnw.cmd -B clean verify'
            }
        }

        stage('Archive JAR') {
            steps {
                archiveArtifacts(
                    artifacts: 'target/*.jar',
                    fingerprint: true
                )
            }
        }
    }

    post {
        success {
            echo 'Spring Boot build and tests succeeded!'
        }

        failure {
            echo 'Build failed. Check Console Output.'
        }
    }
}