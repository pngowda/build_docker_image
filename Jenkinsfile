def customImage
def customContainer
pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
              echo 'Starting to build docker image'
              script {
                    customImage = docker.build("my-image:${env.BUILD_ID}")
                }
            }
        }
        stage('Test Docker Image') {
            steps {
                echo 'Starting to test docker image'
                script {
                   customContainer = customImage.run("-p 8081:8081")
                   sleep 10
                   customImage.inside {sh 'curl -i http:///127.0.0.1:8081/'}
                }
            }
        }
    }
    
    post {
        always {
            echo "Stop Docker image"
            script {
                    if (customContainer) {
                    customContainer.stop()
                    sh "docker rmi -f my-image:${env.BUILD_ID}"
                }
           }
        }
    }
}
