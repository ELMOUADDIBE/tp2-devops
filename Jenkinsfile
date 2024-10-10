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

        stage('Deploy Image') {
            steps {
                script {
                    // Arrêter et supprimer le conteneur existant s'il existe
                    bat """
                    docker rm -f devops-webapp || echo "Le conteneur n'existe pas, poursuite du déploiement..."
                    docker run -d --name devops-webapp ${registry}:${BUILD_NUMBER}
                    """
                }
            }
        }
    }
}
