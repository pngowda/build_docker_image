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
    }
}
