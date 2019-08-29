pipeline {
    agent any
    def customImage
    def customContainer
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
                    customContainer = customImage.run("-d -p 8082:8081 --name test_nexus_container")
                }
            }
        }
    }
}
