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
    }
}
//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