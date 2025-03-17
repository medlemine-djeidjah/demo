pipeline {
    agent any
    
    tools {
        // Define the Maven tool installation to use
        maven 'Maven 3'
        jdk 'JDK 17'
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Explicitly checkout from GitHub
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']], // You can change this to your default branch
                    extensions: [],
                    userRemoteConfigs: [[
                        credentialsId: 'github-credentials', // Jenkins credentials ID for GitHub
                        url: 'https://github.com/medlemine-djeidjah/demo.git' // Replace with your GitHub repo URL
                    ]]
                ])
            }
        }
        
        stage('Build') {
            steps {
                // Run Maven build
                sh 'mvn clean compile'
            }
        }
        
        stage('Test') {
            steps {
                // Run Maven tests
                sh 'mvn test'
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
                // Package the application
                sh 'mvn package -DskipTests'
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