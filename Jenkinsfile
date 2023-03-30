pipeline{
    agent any 
    environment{
        DOCKERHUB_USERNAME ="anuragjoshi01"
        APP_NAME = "node-js-demo"
        IMAGE_TAG ="${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
        REGISTRY_CREDS = "dockerhub"

    }
    stages{
        stage("Checkout from SCM"){
            steps{
                script{
                        git credentailsiD: 'github',
                        url: 'https://github.com/anuragjos/reddit-clone-k8s-ingress.git',
                        branch: 'master'
                }
            }
        }
        stage("Docker Build Image "){
            steps{
                script{
                    docker_image = docker.build "${IMAGE_NAME}"
                }
            }
        }
    }
    
}

//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