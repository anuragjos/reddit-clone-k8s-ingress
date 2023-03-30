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
        stage("Docker Image Push to Docker Hub"){
            steps{
                script{
                    docker.withRegistry('',REGISTRY_CREDS){
                    docker_image.push("$BUILD_NUMBER")
                    docker_image.push('latest')
                    }
                }
            }
        }
        stage("Delete Docker Image"){
            steps{
                script{
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
                }

            }
        }
        stage("Updating Kubernetes Deployment file"){
            steps{
                script{
                    sh """
                    cat deployment.yml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yml
                    cat deployment.yml
                    """
                }
            }
        }
        stage("Push to change Deployment file to Git"){
            steps{
                script{
                    sh """
                    git config --global user.name "Anil Joshi"
                    git config --global user.email "joshianurag088@gmail.com"
                    git add deployment.yml
                    git commit -m "updating deployment file
                    """
                    withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                        sh "git push https://github.com/anuragjos/reddit-clone-k8s-ingress.git master"
                    }
                }
            }
        }
    }
    
}

//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