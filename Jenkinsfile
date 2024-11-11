pipeline {
    agent any
    tools {
        nodejs 'NodeJS' // specify the correct NodeJS installation name in Jenkins
    }
    environment {
        SONAR_PROJECT_KEY = 'nodejs-project'
        SONAR_SCANNER_HOME = tool 'SonarQubeScanner'
        DOCKER_HUB_REPO = 'samuel78996/nodejs-project'
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
        stage ('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('SonarQube') {
                         sh """
                         ${SONAR_SCANNER_HOME}/bin/sonar-scanner \
						 -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
						 -Dsonar.sources=. \
						 -Dsonar.host.url=http://172.187.168.47:9000/ \
					 	 -Dsonar.login=${SONAR_TOKEN}
                         """
                    }
                }          
            }
        }
        stage('Docker Build Image') {
            steps {
                script {
                    docker.build("${DOCKER_HUB_REPO}:v1")
                }
            }
        }
    }
}