def customImage
def customContainer
pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                echo 'Starting to build docker image'

                script {
                    sh "docker rmi -f my-image:2 my-image:3 my-image:4 my-image:5 my-image:6"
                    //customImage = docker.build("my-image:${env.BUILD_ID}")
                }
            }
        }
        stage('Test Docker Image') {
            steps {
                echo 'Starting to test docker image'

                script {
                    println "test"
                    //customContainer = customImage.run("-p 8083:8081 --name test_nexus_container_${env.BUILD_ID}")
                    //customContainer.inside("curl http://localhost:8082")
                }
            }
        }
    }
    post {
        always {
            echo "Stop Docker image"
            script {
                if (customContainer) {
                    println "test"
                    //customContainer.stop()
                }
            }
        }
    }
}
