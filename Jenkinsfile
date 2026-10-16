pipeline {
    agent any

    tools {
        // This must match the JDK name in your Jenkins Global Tool Configuration.
        // Set this to the JDK 21 (or 17) installation you've configured.
        jdk 'jdk21'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout from SCM (works automatically with multibranch or Pipeline from SCM)
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Use the Maven wrapper included in your project
                sh './mvnw clean package -DskipTests=true'
            }
        }

        stage('Test') {
            steps {
                sh './mvnw test'
            }
            post {
                always {
                    // Publish JUnit test results
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Archive') {
            steps {
                // Archive the built JAR for later use
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        failure {
            echo 'Build failed. Check the console output for details.'
        }
    }
}