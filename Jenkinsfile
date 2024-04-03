pipeline{
    agent {
        label "jenkins-agent"
    }
    tools {
        jdk "Java17"
        maven "Maven3"
    }
    environment {
        APP_NAME = "web_app"
        RELEASE = "1.0.0"
        DOCKER_USER = "asdasdsadads"
        DOCKER_PASS = "secret"
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    }
    stages{
        stage("Cleanup Workspace"){
            steps{
                cleanWs ()
            }
        }

        stage("Checkout from SCM"){
            steps{
                git branch: 'main', credentialsId: 'github', url:'https://github.com/Asdasdsadasddas/web_app'
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
        stage("Build image & upload to dockerhub"){
            steps{
                script {
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }

                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push("latest")
                    }
                }
            }
        }
    }
}