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

                    docker_image = docker.build() "${IMAGE_NAME}"
                }
            }
        }
    }
}


//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