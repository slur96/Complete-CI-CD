pipeline {
    agent any
    
    tools {
        nodejs 'nodejs' // Correctly specifying NodeJS tool
    }
    
    stages {
        stage('GitHub') {
            steps {
                git 'https://github.com/slur96/Complete-CI-CD'
            }
        }
        stage('Unit Test') {
            steps {
                sh 'npm install'  // Install dependencies first
                sh 'npm test'
            }
        }
    }
}