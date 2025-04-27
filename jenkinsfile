pipeline {
    agent any
    environment {
        DOCKERHUB_TOKEN = credentials('docker-hub-credential')
    }
    tools {
        maven 'Default Maven' // This refers to your Maven installation name in Jenkins
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
                    
                        echo "Building, testing, and pushing Docker image on development branch"
                        sh 'mvn clean install'
                        sh 'mvn test'
                        sh "echo ${DOCKERHUB_TOKEN} | docker login -u gowri5877 --password-stdin"
                        sh 'docker build -t gowri5877/k8sexamimag .'
                        sh 'docker push gowri5877/k8sexamimag'
                    
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh "mvn clean verify sonar:sonar -Dsonar.projectKey=Java"
                }
            }
        }

        stage('Deployment'){
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }

    }
}
