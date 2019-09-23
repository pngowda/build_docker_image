def customImage
def customContainer
import groovy.json.JsonSlurperClassic
pipeline {
    agent any
    stages {
        
        stage('Parse Json File') {
            steps {
             script {
                  //def props = parseJsonToMap('test.json')
                  def props = readJSON file: 'test.json'
                  println props['BaseImagePath']
                  println props['TargetImagePath']
                  props.images.each {
                     println "image: $it"
                  }
                }
            }
        }
        
       /* stage('Build Docker Image') {
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
        }*/
    }
    
}

@NonCPS
def parseJsonToMap(String json) {
    final slurper = new JsonSlurperClassic()
    return new HashMap<>(slurper.parseText(json))
}

