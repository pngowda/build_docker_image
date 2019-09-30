import groovy.json.JsonSlurper;
node() {
    stage("checkout and parse json") {
      checkout scm
      def jsonSlurper = new JsonSlurper()
      File fl = new File("${WORKSPACE}/images.json")
      // parse(File file) method is available since 2.2.0
      def obj = jsonSlurper.parse(fl)
      println obj.RepoName
      println obj.images.imageType
        
    }
     stage("parse changesets") {
       def changeLogSets = currentBuild.changeSets
       for (int i = 0; i < changeLogSets.size(); i++) {
         def entries = changeLogSets[i].items
         for (int j = 0; j < entries.length; j++) {
           def entry = entries[j]
           echo "${entry.commitId} by ${entry.author} on ${new Date(entry.timestamp)}: ${entry.msg}"
           def files = new ArrayList(entry.affectedFiles)
           for (int k = 0; k < files.size(); k++) {
              def file = files[k]
              echo "  ${file.editType.name} ${file.path}"
            }
         }
      }
    }
    
    stage('define version info') {
            echo "current build number: ${currentBuild.number}"
            echo "previous build number: ${currentBuild.previousBuild.getNumber()}"
            def causes = currentBuild.rawBuild.getCauses()
            echo "causes: ${causes}"
    }
}
