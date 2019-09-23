node() {
    stage("checkout") {
        def jsonString = '{"RepoName": "test", \
        "images": 
        [ \
            {\
               "imageType": "base", \
               "imageName": "",\
               "imagePath": "./base/Dockerfile",\
               "imageRunCmd": "",\
               "imageTestCmd": "",\
               "imageVersion": "",\
               "dependsOn":"base"\
        }\
        ]}'
        def jsonObj = readJSON text: jsonString

        assert jsonObj['RepoName'] == 'test'  // this is a comparison.  It returns true
        sh "echo ${jsonObj.RepoName}"  // prints out katone
    }
}
