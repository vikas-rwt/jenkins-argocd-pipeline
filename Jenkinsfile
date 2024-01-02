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
        stage('Trigger CD pipeline'){
            steps{
                script{

                    sh "curl -v -k -user admin-11482c489c56545d9cbc655e482922677e -X POST -H 'cache-control: no-cache' -H 'content-type: application/x-www-form-urlencoded' -data 'IMAGE_TAG=${IMAGE_TAG}' 'http://54.172.171.86:8080/job/gitops/buildWithParameters?token=gitops-token"
                }
            }
        }
    }
}


//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
//11482c489c56545d9cbc655e482922677e