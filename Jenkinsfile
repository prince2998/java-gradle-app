  pipeline{
      agent any
      environment{
          VERSION = "${env.BUILD_ID}"
          DOCKER_HOSTED_EP = "13.235.91.151:8083" 
      }
      stages{
          stage("Sonar Quality Check"){
              steps{
                  script{
                      withSonarQubeEnv(credentialsId: 'sonar-token') {
                          sh 'chmod +x gradlew'
                          sh './gradlew sonarqube --info'
                      }
                      timeout(time: 1, unit: 'MINUTES') {
                          def qg = waitForQualityGate()
                          if (qg.status != 'OK') {
                              error "Pipeline aborted due to quality gate failure: ${qg.status}"
                          }
                      }
                  }
              }
          }
          stage("Build docker images and push to Nexus"){
              steps{
                  script{
                      withCredentials([string(credentialsId: 'nexus_pass', variable: 'nexus_pass_var')]) {
                          sh '''
                          docker build -t $DOCKER_HOSTED_EP/javawebapp:${VERSION} .
                          docker login -u admin -p $nexus_pass_var $DOCKER_HOSTED_EP
                          docker push $DOCKER_HOSTED_EP/javawebapp:${VERSION}
                          docker rmi $DOCKER_HOSTED_EP/javawebapp:${VERSION}
                          '''
                      }
                  }
              }
          }
      }            
  }




