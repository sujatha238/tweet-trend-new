pipeline {
    agent {
        node {
          label 'maven'
        }
    }
    /*tools {
        // Use Java 8 for the build
        //JDK 'JAVA_HOME_11'
        //jdk 'JAVA_HOME_11'

    }*/
environment {
    jdk = tool 'JAVA_HOME_11'
}
environment {
    PATH = "/opt/apache-maven-3.9.6/bin:$PATH"
}
    stages {
        stage("build"){
            steps {
                 echo "----------- build started ----------"
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                 echo "----------- build completed ----------"
            }
        }
        stage("test"){
            steps{
                echo "----------- unit test started ----------"
                sh 'mvn surefire-report:report'
                 echo "----------- unit test completed ----------"
            }
        }

        stage('SonarQube analysis') {
          /*tools {

            //JDK 'JAVA_HOME_17'
            jdk 'JAVA_HOME_17'
         }*/
        environment {
          jdk = tool 'JAVA_HOME_17'
        
          scannerHome = tool 'new-sonar-scanner'
        }
          steps{
          withSonarQubeEnv('new-sonarserver') { // If you have configured more than one global server connection, you can specify its name
            sh "${scannerHome}/bin/sonar-scanner"
        }
      }
    }
        stage("Quality Gate"){
          steps {
          script {
          timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
          def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
        if (qg.status != 'OK') {
        error "Pipeline aborted due to quality gate failure: ${qg.status}"
    }
  }
  }//def registry = 'https://valaxy01.jfrog.io'
    }
  }
  }   
}     /*stage("Jar Publish") {
          steps {
            script {
                    echo '<--------------- Jar Publish Started --------------->'
                     def server = Artifactory.newServer url:registry+"/artifactory" ,  credentialsId:"artfactcred"                    
                     def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}";
                     def uploadSpec = """{
                          "files": [
                            {
                              "pattern": "jarstaging/(*)",
                              "target": "abc-libs-release-local/{1}",
                              "flat": "false",
                              "props" : "${properties}",
                              "exclusions": [ "*.sha1", "*.md5"]
                            }
                         ]
                     }"""
                     def buildInfo = server.upload(uploadSpec)
                     buildInfo.env.collect()
                     server.publishBuildInfo(buildInfo)
                     echo '<--------------- Jar Publish Ended --------------->'  
            
              }  
            }
        }
        stage(" Docker Build ") {
          steps {
            script {
               echo '<--------------- Docker Build Started --------------->'
               app = docker.build(imageName+":"+version)
               echo '<--------------- Docker Build Ends --------------->'
        }
      }
    }

        stage (" Docker Publish "){
          steps {
            script {
               echo '<--------------- Docker Publish Started --------------->'  
                docker.withRegistry(registry, 'artfactcred'){
                    app.push()
                }    
               echo '<--------------- Docker Publish Ended --------------->'  
            }
          }
        }
        stage ("Deploy"){
          steps{
            script{
              sh './deploy.sh'
            }
          }
        }
    }
            
  }*/   
