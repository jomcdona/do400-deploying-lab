pipeline {
      agent {
         node {
            label "maven"
         }
      }
      environment { QUAY = credentials('QUAY_USER') }
 
      stages {
          stage("Test") {
             steps {
                 sh "./mvnw verify"
             }
          }

          stage("Build & Push Image") {
             steps {
                 sh './mvnw quarkus:add-extension -Dextensions="container-image-jib"'

                 sh '''
                     ./mvnw package -DskipTests \
                         -Dquarkus.container-image.build=true \
                         -Dquarkus.container-image.registry=quay.io \
                         -Dquarkus.container-image.group=$QUAY_USR \
                         -Dquarkus.container-image.name=do400-deploying-lab \
                         -Dquarkus.container-image.username=$QUAY_USR \
                         -Dquarkus.container-image.password=$QUAY_PSW \
                         -Dquarkus.container-image.push=true
                 '''
             }
          }
         
          stage("Deploy To Test) {
             when { not { branch "main" } }

             steps {
                sh '''
                     oc rollout latest dc/home-automation -n jomcdona-deploying-lab-test
                '''
              }
          }

      }
}
