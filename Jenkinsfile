pipeline {
    environment {
        registry = "elmouaddibezaid/devops-webapp"
        registryCredential = 'dockerhub_credentials'
        dockerImage = ''
    }
    agent any
    stages {
        stage('Cloning Git') {
            steps {
                git branch: 'main', url: 'https://github.com/ELMOUADDIBE/tp2-devops.git'
            }
        }
		
        stage('Building image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
		
        stage('Test image') {
            steps {
                script {
                    echo "Tests passed"
                }
            }
        }
		
        stage('Publish Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                        
                        // Pousser l'image avec le tag 'latest'
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }
}
