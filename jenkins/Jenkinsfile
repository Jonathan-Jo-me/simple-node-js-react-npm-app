pipeline {
    agent any
    environment {
        DISCORD_WEBHOOK_URL = 'https://discord.com/api/webhooks/1291652946894782565/Z2DLNdFmdQfLJ2dUX5kBAgIGEdax7Gz-VWICJvHCcMHVXqC0hIlbwvWGMy8ICpfuUeNj'  // Replace with your webhook URL
    }
    stages {
        stage('Clean Workspace') {
            steps {
                // Remove the node_modules directory to avoid conflicts
                sh 'rm -rf node_modules'
            }
        }

        stage('Build') { 
            steps {
                // Install dependencies
                sh 'npm install'
            }
        }
        
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
    post {
        success {
            script {
                def message = "✅ Build succeeded: ${env.JOB_NAME} #${env.BUILD_NUMBER}"
                sh "curl -H 'Content-Type: application/json' -d '{\"content\": \"${message}\"}' ${DISCORD_WEBHOOK_URL}"
            }
        }
        failure {
            script {
                def message = "❌ Build failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}"
                sh "curl -H 'Content-Type: application/json' -d '{\"content\": \"${message}\"}' ${DISCORD_WEBHOOK_URL}"
            }
        }
    }
}
