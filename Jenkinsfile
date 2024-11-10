pipeline {
    agent { 
        node {
            label 'docker-node'
        }
    }

    environment {
		SONAR_PROJECT_KEY = 'full-project'
		SONAR_SCANNER_HOME = tool 'SonarQubeScanner'
    }
    
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
        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'full-project', variable: 'SONAR_TOKEN')]) {

                    	withSonarQubeEnv('SonarQube') {
    						sh """
						${SONAR_SCANNER_HOME}/bin/sonar-scanner \
						-Dsonar.projectKey=${SONAR_PROJECT_KEY} \
						-Dsonar.sources=. \
						-Dsonar.host.url=http://localhost:9000 \
						-Dsonar.login=${SONAR_TOKEN}
						"""
					}
                }
            }
        }
    }
}