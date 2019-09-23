import groovy.json.JsonSlurper
node() {
    stage("checkout") {
        checkout scm
       def jsonSlurper = new JsonSlurper()
       data = jsonSlurper.parse(new File("${env.WORKSPACE}/test.json"))
 
       println(data)
    }
}
