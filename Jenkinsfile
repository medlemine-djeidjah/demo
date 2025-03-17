pipeline {
    agent any
    
    // Removing specific tool names since they're not configured in Jenkins
    
    stages {
        stage('Checkout') {
            steps {
                // Use simple checkout without credentials
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    extensions: [],
                    userRemoteConfigs: [[
                        url: 'https://github.com/medlemine-djeidjah/demo.git'
                        // Removed credentialsId since it's not configured
                    ]]
                ])
            }
        }
        
        stage('Build') {
            steps {
                // Use Maven wrapper instead of system Maven
                sh 'chmod +x ./mvnw'
                sh './mvnw clean compile'
            }
        }
        
        stage('Test') {
            steps {
                // Use Maven wrapper for tests
                sh './mvnw test'
            }
            post {
                always {
                    // Publish test results
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }
        
        stage('Package') {
            steps {
                // Use Maven wrapper for packaging
                sh './mvnw package -DskipTests'
            }
            post {
                success {
                    // Archive the built artifacts
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                }
            }
        }
    }
} 