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
                    docker.image("my-image:${env.BUILD_ID}").withRun('-p 8081:8081') {c ->
                       sh "curl -i http://${hostIp(c)}:8081/"
                    }
               
                    //customContainer = customImage.run("-p 8083:8081 --name test_nexus_container_${env.BUILD_ID}")
                    //customImage.inside {sh 'curl http://localhost:8083/'}
                }
            }
        }
    }
    
    
    post {
        always {
            echo "Stop Docker image"
            script {
                sh "docker rmi -f my-image:${env.BUILD_ID}"
                if (customContainer) {
                    customContainer.stop()
                    sh "docker rmi -f my-image:${env.BUILD_ID}"
                }
            }
        }
    }
}

def hostIp(container) {
  sh "docker inspect -f {{.Node.Ip}} ${container.id} > hostIp"
  readFile('hostIp').trim()
}
