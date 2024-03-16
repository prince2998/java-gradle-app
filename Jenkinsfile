  pipeline{
      agent any
      environment{
          VERSION = "{env.BUILD_ID}"
      }
      stages{
          stage("Sonar Quality Check"){
              steps{
                  script{
                      withSonarQubeEnv(credentialsId: 'sonar-token') {
                          sh 'chmod +x gradlew'
                          sh './gradlew sonarqube --info'
                      }
                  }
              }
          }
      }
  }


