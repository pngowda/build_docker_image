import groovy.json.JsonSlurper;
def base_build_version=""
node() {
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
         base_build_version=obj.images."${image}".imageVersion
      }
      //println obj.images.base.imagePath
        
    }
     stage("parse changesets") {
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
      def buildBaseRequired=false
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
    //stage('build base image') {
      //  echo "${env.USER}"
       // dir("${env.WORKSPACE}/base"){
        //sh "pwd"
        //baseImage = docker.build("baseimage:${env.BUILD_ID}")
        //}
    //}
    stage('build target image') {
       dir("${env.WORKSPACE}/target"){
       sh "pwd"
       sh 'docker build --build-arg version="${base_build_version}" .'
       // targetImage = docker.build("targetimage:${env.BUILD_ID}")
      }
    }
}
