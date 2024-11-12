pipeline {
    agent any
    tools {
        nodejs 'NodeJS' 
    }

    environment {
        SONAR_PROJECT_KEY = 'nodejs-project'
        SONAR_SCANNER_HOME = tool 'SonarQubeScanner' 
        DOCKER_HUB_REPO = 'samuel78996/nodejs-project'
        JOB_NAME_NOW = 'node-prod'
        REGISTRY_NAME = "samuel78996nodeapp.azurecr.io"
        IMAGE_NAME = "node-app"
        IMAGE_TAG = "v1.0.${BUILD_NUMBER}"
        REGISTRY_CREDENTIALS = credentials('acr_id')
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
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('SonarQube') { 
                        sh '''${SONAR_SCANNER_HOME}/bin/sonar-scanner \
                             -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                             -Dsonar.sources=. \
                             -Dsonar.host.url=http://20.90.209.96:9000/ \
                             -Dsonar.login=${SONAR_TOKEN}
                        '''
                    }
                }
            }
        }

        stage('Docker Image') {
            steps {
                script {
                    docker.build("${REGISTRY_NAME}/${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }

        stage('Login to ACR') {
            steps {
                script {
                    // Login to ACR
                    sh "docker login ${REGISTRY_NAME} -u ${REGISTRY_CREDENTIALS_USR} -p ${REGISTRY_CREDENTIALS_PSW}"
                }
            }
        }

        stage('Trivy Scan') {
            steps {
                sh 'trivy --severity HIGH,CRITICAL --no-progress --format table -o trivy-report.html image ${JOB_NAME_NOW}:latest'
            }
        }
    }
}