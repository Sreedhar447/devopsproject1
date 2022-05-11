pipeline {
    environment { 
        registry = "sreedhar98765/devopsproject1" 
        registryCredential = 'docker-hub' 
        dockerImage = '' 
    }
    agent any

    stages {
        
        stage('Build Image') {
            steps {
                echo 'Building Docker Image'
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        
        stage('Deploy Image') {
            steps {
                echo 'Pushing Docker Image'
                script {
                   docker.withRegistry( '', registryCredential ) {
                   dockerImage.push("$BUILD_NUMBER")
                   dockerImage.push('latest')
                  }
                }
            }
        }
        
        stage('Clean Up') {
            steps {
                sh "docker rmi $registry:$BUILD_NUMBER"
                sh "docker rmi $registry:latest"
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
                sh "kubectl apply -f kuberantes-deployment-manifest.yml"
                sh "kubectl apply -f service.yml"
                sh "kubectl rollout restart deployment.apps/calc-deployment"
            }
        }
    }
}
