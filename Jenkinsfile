pipeline {
      agent {
         label "maven"
      }
 
      stages {
          stage("Test") {
              sh "./mvnw verify"
          }
      }
}
