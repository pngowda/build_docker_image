import groovy.json.JsonSlurper;

node() {
    def base_build_version
    def buildBaseRequired=false
    def imageList
    def obj
    /************************************************************
    
    ************************************************************/
     stage("checkout and parse json") {
       checkout scm
       def jsonSlurper = new JsonSlurper()
       File fl = new File("${WORKSPACE}/images.json")
       obj = jsonSlurper.parse(fl)
       imageList=obj.images.keySet() 
       imageList.each{image->
         println image
         println obj.images."${image}".dependsOn
         println obj.images."${image}".imagePath
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
    stage('define version info') {
            echo "current build number: ${currentBuild.number}"
            echo "previous build number: ${currentBuild.previousBuild.getNumber()}"
            def causes = currentBuild.rawBuild.getCauses()
            echo "causes: ${causes}"
    }
    
    stage ('docker build') {
       imageList.each{image->
           def dependentImage=obj.images."${image}".dependsOn
           println "dependednt image "+ dependentImage
           //if(obj.images."${image}".dependsOn != null ){
             //  def dependentImage=obj.images."${image}".dependsOn
              // println dependentImage
               //build job: 'pipelineA', parameters: [string(name: 'param1', value: "value1")]
           //}
       }
    }
    
    /************************************************************
    
    ************************************************************/
   /* stage('build base image') {
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
                
    }*/
   
    /************************************************************
    
    ************************************************************/
    /*stage('build target image') {
       dir("${env.WORKSPACE}/target"){
         sh "docker build -t prajwaln22/targetimage:${env.BUILD_ID} --build-arg BASEIMAGE=prajwaln22/baseimage --build-arg VERSION=${base_build_version} . "
         docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
              sh "docker push prajwaln22/targetimage:${env.BUILD_ID}"
          }
       }
    }
    stage('Remove Unused docker image') {
      sh "docker rmi prajwaln22/targetimage:${env.BUILD_ID}"
      if(buildBaseRequired) {
         sh "docker rmi prajwaln22/baseimage:${env.BUILD_ID}"
      }
    }*/
}
