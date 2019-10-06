import groovy.json.JsonSlurper;
import groovy.json.JsonBuilder;

node() {
    def base_build_version
    def buildBaseRequired=false
    def imageList
    def jsonSlurper
    /************************************************************
    
    ************************************************************/
     stage("checkout and parse json") {
       checkout scm
       jsonSlurper = new JsonSlurper()
       File fl = new File("${WORKSPACE}/images.json")
       def obj = jsonSlurper.parse(fl)
       println (obj)
       imageList=obj.images.keySet() 
       imageList.each{image->
          base_build_version=obj.images.base.imageVersion
       }
     }
    
    /************************************************************
    
    ************************************************************/
     stage("parse changesets") {
       def changeLogSets = currentBuild.changeSets
       def modifiedList=[]
       for (int i = 0; i < changeLogSets.size(); i++) {
         def entries = changeLogSets[i].items
         for (int j = 0; j < entries.length; j++) {
           def entry = entries[j]
           def files = new ArrayList(entry.affectedFiles)
           for (int k = 0; k < files.size(); k++) {
              def file = files[k]
              modifiedList.add("${file.path}")
            }
         }
       }
       def pattern = /base\/.*/   
       modifiedList.each{filepath->
          if(("${filepath}").contains("base")){
              println "base is modified"
              buildBaseRequired=true
          }
       }
    }
    
   /************************************************************
    
   ************************************************************/
   stage('build base image') {
        if(buildBaseRequired) {
          dir("${env.WORKSPACE}/base"){
            sh "docker build -t prajwaln22/baseimage:${env.BUILD_ID} ."
            base_build_version="${env.BUILD_ID}"
            docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
              sh "docker push prajwaln22/baseimage:${env.BUILD_ID}"
            }
          }
        }
         else{
           println "base image to be taken from the registry" 
         }
       
       //File fl = new File("${WORKSPACE}/images.json")
       //def obj = jsonSlurper.parse(fl)
       //def builder = new JsonBuilder(obj)
       //builder.content.device.dpidsha1 = 'abcd'  
       //println(builder.toPrettyString())
     }
   
   /************************************************************
    
   ************************************************************/
    stage('build target image') {
       dir("${env.WORKSPACE}/target"){
         sh "docker build -t prajwaln22/targetimage:${env.BUILD_ID} --build-arg BASEIMAGE=prajwaln22/baseimage --build-arg VERSION=${base_build_version} . "
         docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
              sh "docker push prajwaln22/targetimage:${env.BUILD_ID}"
          }
       }
     }
    
   /************************************************************
    
   ************************************************************/
    stage('Remove Unused docker image') {
      sh "docker rmi prajwaln22/targetimage:${env.BUILD_ID}"
      if(buildBaseRequired) {
          sh "docker rmi prajwaln22/baseimage:${env.BUILD_ID}"
       }
     }
  }
