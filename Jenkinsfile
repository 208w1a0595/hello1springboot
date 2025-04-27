pipeline {
    agent any
     environment {
    DOCKERHUB_TOKEN = credentials('docker-hub-credential')
  }
    stages {
	
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Test') {
            steps {
                script {
                    if (env.BRANCH_NAME.startsWith('feature/')) {
                        echo "Running tests on feature branch"
                        sh 'mvn clean install'
                        sh 'mvn test'
                    } 
                    }
                }
            }
	
        }
	
    }
