pipeline {
    agent any
    
    // Auto-detects pushes from GitHub every 2 minutes
    triggers {
        pollSCM('H/2 * * * *')
    }
    
    tools { 
        nodejs 'node20' 
    }
    
    // Connects containers using Docker network names instead of localhost
    environment {
        SELENIUM_REMOTE_URL = 'http://selenium:4444/wd/hub'
        APP_URL = 'http://jenkins:3000'
    }
    
    stages {
        stage('Install') { 
            steps { 
                sh 'npm install' 
            } 
        }
        
        stage('Unit Test') { 
            steps { 
                sh 'npm test' 
            } 
        }
        
        stage('UI Test') {
            steps {
                sh 'npx jest tests/e2e'
            }
        }
    }
    
    // Publishes test results so Jenkins can track them over time
    post {
        always {
            junit 'junit.xml'
        }
    }
}