node() {
    stage("checkout") {
        def jsonString = '{"name":"katone","age":5}'
        def jsonObj = readJSON text: jsonString

        assert jsonObj['name'] == 'katone'  // this is a comparison.  It returns true
        sh "echo ${jsonObj.name}"  // prints out katone
        sh "echo ${jsonObj.age}"   // prints out 5
    }
}
