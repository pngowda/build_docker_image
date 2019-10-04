import groovy.json.JsonSlurper;

node() {
    environment {
      registry = "prajwaln22/targetimage"
      registryCredential = 'dockerhub'
      dockerImage = ''
    }
    def base_build_version
    def buildBaseRequired=false
    stage("checkout and parse json") {
      checkout scm
      def jsonSlurper = new JsonSlurper()
      File fl = new File("${WORKSPACE}/images.json")
      // parse(File file) method is available since 2.2.0
      def obj = jsonSlurper.parse(fl)
      println obj.RepoName
      //println obj.images.keySet() 
      def imageList=obj.images.keySet() 
      imageList.each{image->
         println obj.images."${image}".imagePath
         println obj.images."${image}".imageVersion
         base_build_version=obj.images.base.imageVersion
         println "base version "+base_build_version
      }
      //println obj.images.base.imagePath
        
    }
     stage("parse changesets") {
       println "base version "+base_build_version
       def changeLogSets = currentBuild.changeSets
       def modifiedList=[]
       for (int i = 0; i < changeLogSets.size(); i++) {
         def entries = changeLogSets[i].items
         for (int j = 0; j < entries.length; j++) {
           def entry = entries[j]
           //println "${entry.commitId} by ${entry.author} on ${new Date(entry.timestamp)}: ${entry.msg}"
           def files = new ArrayList(entry.affectedFiles)
           for (int k = 0; k < files.size(); k++) {
              def file = files[k]
              //println "${file.path}"
              modifiedList.add("${file.path}")
            }
         }
      }

      def pattern = /base\/.*/   
      modifiedList.each{filepath->
          println "${filepath}"
          if(("${filepath}").contains("base")){
              println "base is modified"
              buildBaseRequired=true
          }
      }
         println buildBaseRequired
      
  }
    
    stage('define version info') {
            echo "current build number: ${currentBuild.number}"
            echo "previous build number: ${currentBuild.previousBuild.getNumber()}"
            def causes = currentBuild.rawBuild.getCauses()
            echo "causes: ${causes}"
    }
    stage('build base image') {
        if(buildBaseRequired) {
          dir("${env.WORKSPACE}/base"){
            sh "pwd"
            sh "docker build -t baseimage:${env.BUILD_ID} ."
            base_build_version="${env.BUILD_ID}"
          }
        }
         else{
           println "base image to be taken from the registry" 
         }
                
    }
    stage('build target image') {
       println base_build_version
        
       dir("${env.WORKSPACE}/target"){
         sh "pwd"
         sh "docker build -t prajwaln22/targetimage:${env.BUILD_ID} --build-arg BASEIMAGE=baseimage --build-arg VERSION=${base_build_version} . "
         sh "docker push prajwaln22/targetimage:${env.BUILD_ID}"
       }
    }
    
   // stage('Push image') {
     //   docker.withRegistry('https://hub.docker.com/', 'dockerhub') {
       //    sh "docker push prajwaln22/targetimage:${env.BUILD_ID}"
        //}
    //}
}
