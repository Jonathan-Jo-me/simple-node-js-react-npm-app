pipeline {
    agent any
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
}
