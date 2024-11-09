pipeline {
	agent any
	tools {
		nodejs 'NodeJS'
	}
	
	}
	stages {
		stage('GitHub'){
			steps {
				git 'https://github.com/slur96/Complete-CI-CD'
			}
		}
		stage('Unit Test'){
			steps {
				sh 'npm test'
				sh 'npm install'		
			}
		}
	}
