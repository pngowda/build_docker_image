node() {
    stage("checkout") {
       def jsonSlurper = new JsonSlurper()
       data = jsonSlurper.parse(new File('test.json'))
 
       println(data)
    }
}
