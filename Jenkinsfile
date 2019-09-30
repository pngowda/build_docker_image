import groovy.json.JsonSlurper;
node() {
    stage("checkout") {
      checkout scm
      def jsonSlurper = new JsonSlurper()
      File fl = new File("${WORKSPACE}/images.json")
      // parse(File file) method is available since 2.2.0
      def obj = jsonSlurper.parse(fl)
      println obj.RepoName
      println obj.images.imageType
      //def changeSet = getChangeSet()
      //print changeSet
    }
    def changeLogSets = currentBuild.rawBuild.changeSets
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

@NonCPS

// Fetching change set from Git
def getChangeSet() {
  return currentBuild.changeSets.collect { cs ->
    cs.collect { entry ->
        "* ${entry.author.fullName}: ${entry.msg}"
    }.join("\n")
  }.join("\n")
}
