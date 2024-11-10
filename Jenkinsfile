pipeline {
    agent any
    
    tools {
        tool name: 'NodeJS' // specify the correct NodeJS installation name in Jenkins
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