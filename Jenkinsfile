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
                    println customImage
                }
            }
        }
        stage('Test Docker Image') {
            steps {
                echo 'Starting to test docker image'

                script {
                    customContainer = customImage.run("-p 8082:8081 --name test_nexus_container_${env.BUILD_ID}")
                    customContainer.inside("curl http://localhost:8082")
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
