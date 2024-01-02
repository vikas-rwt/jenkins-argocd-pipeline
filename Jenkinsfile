pipeline{

    agent any

    environment{

        DOCKERHUB_USERNAME = "vikasrwt"
        APP_NAME = "jenkins-argocd"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
        REGISTRY_CREDS = 'dockerhub'
    }

    stages{

        stage('Cleanup workspace'){

            steps{
                script{
                    cleanWs()
                }
            }
        }
        stage('Checkout SCM'){

            steps{
                script{
                    git credentialsId: 'github',
                    url: 'https://github.com/vikas-rwt/jenkins-argocd-pipeline.git',
                    branch: 'main'
                }
            }
        }
        stage('Docker Build'){
            steps{
                script{
                    docker_image = docker.build "${IMAGE_NAME}"
                }
            }
        }
        stage('Push Docker Image'){
            steps{
                script{
                    
                    docker.withRegistry('',REGISTRY_CREDS){
                        docker_image.push("${BUILD_NUMBER}")
                        docker_image.push('latest')
                    }
                }
            }
        }
        stage('Delete Docker images'){
            steps{
                script{

                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
                }
            }
        }
        stage('Updating Kubernetes Deployment File'){
            steps{
                script{
                    sh"""
                    cat deployment.yml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yml
                    cat deployment.yml
                    """
                }
            }
        }
        stage('Update and push deployment file to Github'){
            steps{
                script{
                    sh"""
                    git config --global user.name "vikas-rwt"
                    git config --global user.email "vrwt22@gmail.com"
                    git add deployment.yml
                    git commit -m "Updated deployment file"
                    """
                    withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                        sh "git push https://github.com/vikas-rwt/jenkins-argocd-pipeline.git main"
                    }
                }
            }
        }
    }
}


//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