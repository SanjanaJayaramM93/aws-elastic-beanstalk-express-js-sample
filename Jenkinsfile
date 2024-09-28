pipeline {
    agent {
        docker {
            image 'node:16'
            label 'node-16'
        }
    }
    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    // Install dependencies using npm
                    sh 'npm install --save'
                }
            }
        }
        stage('Security Scan') {
            steps {
                script {
                    // Run Snyk security scan
                    sh 'snyk test --all-projects'
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    // Add build commands if necessary
                    echo 'Building the application...'
                }
            }
        }
    }
    post {
        always {
            // Archive artifacts (optional)
            archiveArtifacts artifacts: '**/dist/**/*', fingerprint: true
        }
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}

