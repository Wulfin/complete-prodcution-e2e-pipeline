pipeline{
    agent{
        label "jenkins-agent"
    }
    tools{
        jdk 'Java17'
        maven 'Maven3'
    }
    environment{
        APP_NAME = "complete-prodcution-e2e-pipeline"
        RELEASE = "1.0.0"
        DOCKER_USER = "saifffff"
        DOCKER_PASS = "dockerhub"
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}" 
    }
    stages{
        stage("Cleanup workspace"){
            steps{
                cleanWs()
            }
        }

        stage("Checkout from SCM"){
            steps{
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/Wulfin/complete-prodcution-e2e-pipeline'
            }
        }

        stage("Build app"){
            steps{
                sh "mvn clean package"
            }
        }

        stage("Test app"){
            steps{
                sh "mvn test"
            }
        }

        stage("Build and Push Docker image"){
            steps{
                script{
                    docker.withRegistry('', DOCKER_PASS){
                    docker_image = docker.build "${IMAGE_NAME}"
                }

                docker.withRegistry('', DOCKER_PASS){
                    docker_image.push("${IMAGE_TAG}")
                }
                }
            }
        }
    }
}