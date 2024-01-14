pipeline{
    agent any

    environment{
        DOCKERHUB_USERNAME="vanshulsuneja98415"
        APP_NAME = "gitops-argo-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}/${APP_NAME}"
        REGISTRY_CREDS = 'dockerhub'
    }
    stages{
        stage('Clean  Workspace'){
             steps{
                script{
                    cleanWs()
                }
             }
        }
         stage('Checkout Code from Git') {
            steps {
                script {
                    git credentialsId: 'github',
                        url: 'https://github.com/Vanshul97/gitops_argocd.git',
                        branch: 'main'
                        
                }
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    docker_image=docker.build "${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }
        stage('Push Docker Image to DockerHub'){
            steps{
                script{
                    docker.withRegistry('https://registry.hub.docker.com',REGISTRY_CREDS){
                        docker_image.push("$BUILD_NUMBER")
                        docker_image.push('latest')
                    }
                }
            }
        }
        stage('Delete Docker Images') {
            steps {
                script {
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    // sh "docker rmi ${IMAGE_NAME}:latest"
                    
                }
            }
        }
        stage('Update Kubenertes Deployment file') {
            steps {
                script {
                    sh """
                    cat deployment.yml
                    sed -i "s/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g" deployment.yml
                    cat deployment.yml
                    """
                    
                }
            }
        }
    }
}
//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