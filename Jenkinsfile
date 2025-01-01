pipeline {
    agent any
    tools {
        nodejs 'nodejs-22.12.0'  // This refers to the Jenkins tool configuration for Node.js
    }
 
    environment {
        NODEJS_HOME = 'C:/Program Files/nodejs'  // You can also manage this via Jenkins Node.js configuration
        SONAR_SCANNER_PATH = 'C:/Users/karan/Downloads/sonar-scanner-cli-6.2.1.4610-windows-x64/sonar-scanner-6.2.1.4610-windows-x64/bin'
    }
 
    stages {
        stage('Checkout') {
            steps {
                checkout scm  // Check out the latest code from your version control system
            }
        }
 
        stage('Install Dependencies') {
            steps {
                // Install Node.js dependencies
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                npm install
                '''
            }
        }
 
        stage('Lint') {
            steps {
                // Run linting and fix automatically where possible
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                npm run lint 
                '''
            }
        }
 
        stage('Build') {
            steps {
                // Run the build script to create production-ready build
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                npm run build
                '''
            }
        }
 
        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonar-token')  // Retrieve SonarQube token from Jenkins credentials store
            }
            steps {
                // Ensure sonar-scanner is available in the path and perform SonarQube analysis
                bat '''
                set PATH=%SONAR_SCANNER_PATH%;%PATH%
                where sonar-scanner || echo "SonarQube scanner not found. Please install it."
                sonar-scanner -Dsonar.projectKey=sonar-coverage ^
                    -Dsonar.sources=. ^
                    -Dsonar.host.url=http://localhost:9000 ^
                    -Dsonar.token=%SONAR_TOKEN% 2>&1
                '''
            }
        }
    }
 
    post {
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
        always {
            echo 'This runs regardless of the result.'
        }
    }
}
