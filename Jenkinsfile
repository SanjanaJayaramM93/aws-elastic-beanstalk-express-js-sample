pipeline {
    agent {
        docker {
            image 'node:16' // Use Node 16 Docker image as the build agent
        }
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                // Install project dependencies
                sh 'npm install --save'
            }
        }
        stage('Snyk Security Scan') {
            steps {
                echo 'Scanning for vulnerabilities...'
                snykSecurity(
                    snykInstallation: 'Snyk', // Name you provided when configuring Snyk installation
                    snykTokenId: '24655fce-04fb-4cd2-ba46-377a974932ce', // Your Snyk API Token ID
                    failOnIssues: true, // Fail the build if vulnerabilities are found
                    monitorProjectOnBuild: true, // Monitor the project on every build
                    severity: 'high' // Minimum severity to detect
                )
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
                // Add your deploy steps here
            }
        }
    }

    post {
        failure {
            echo 'Build failed due to vulnerabilities detected!'
        }
    }
}

