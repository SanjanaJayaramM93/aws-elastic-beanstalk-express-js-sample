pipeline {
    agent {
        docker {
            image 'node:16'
            args '--privileged -v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    stages {
        stage('Environment Setup') {
            steps {
                echo 'Setting up the environment...'
                sh 'echo "Current Environment Variables:" && env'
                sh 'echo "Working Directory:" && pwd'
                sh 'echo "Contents of Workspace:" && ls -al /var/jenkins_home/workspace'
                sh 'echo "Current User:" && whoami'
            }
        }
        stage('Verify Node.js and npm Installation') {
            steps {
                echo 'Verifying installations of Node.js and npm...'
                sh 'node --version || echo "Node.js is not installed!"'
                sh 'npm --version || echo "npm is not installed!"'
            }
        }
        stage('Source Code Checkout') {
            steps {
                echo 'Checking out the application code from GitHub repository...'
                git url: 'https://github.com/SanjanaJayaramM93/aws-elastic-beanstalk-express-js-sample.git', branch: 'main'
                echo 'Code checkout complete.'
            }
        }
        stage('Validate package.json Presence') {
            steps {
                echo 'Validating presence of package.json...'
                script {
                    if (!fileExists('package.json')) {
                        error 'Error: package.json not found! Ensure it is present in the repository.'
                    } else {
                        echo 'package.json is found, proceeding with the installation.'
                    }
                }
            }
        }
        stage('Install Project Dependencies') {
            steps {
                echo 'Installing project dependencies using npm...'
                sh 'npm install --save'
                echo 'Dependencies installed successfully.'
            }
        }
        stage('Snyk Security Assessment') {
            steps {
                echo 'Initiating Snyk security scan for vulnerabilities...'
                sh 'npm install -g snyk'
                
                // Directly using the Snyk token for authentication
                sh 'snyk auth 54e4355f-5c0c-41fd-aae4-7bf34efedbc3'
                echo 'Snyk authentication successful.'
                
                script {
                    def snykScanResult = sh(script: 'snyk test --severity-threshold=high', returnStatus: true)
                    if (snykScanResult != 0) {
                        error 'Critical vulnerabilities detected by Snyk! Failing the pipeline.'
                    } else {
                        echo 'Snyk scan completed with no critical vulnerabilities. Proceeding...'
                    }
                }
            }
        }
        /*stage('Run Unit Tests') {
            steps {
                echo 'Executing unit tests...'
                sh 'npm test'
            }
        } */
        stage('Application Build') {
            steps {
                echo 'Building the application...'
                sh 'npm run build'
                echo 'Build completed successfully.'
            }
        }
        stage('Deployment Stage') {
            steps {
                echo 'Starting application deployment...'
                // Add your deployment commands here
                echo 'Deployment process initiated.'
            }
        }
    }
    post {
        always {
            echo 'Pipeline execution finished. Cleaning up resources...'
        }
        failure {
            echo 'Pipeline encountered errors. Please check the logs for details and rectify the issues.'
        }
        success {
            echo 'Pipeline completed successfully! All stages executed without errors.'
        }
    }
}


