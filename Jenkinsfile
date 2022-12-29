pipeline {
  agent any
  stages {
    stage('Deploy') {
      parallel {
        stage('Deploy Staging') {
          steps {
            echo 'Staging'
            script {
              try {
                timeout(time: 60, unit: 'MINUTES') {
                  env.DEPLOY_STAGING = input(
                    message: 'Continue with Deploy to Staging EU?',
                    parameters: [
                      booleanParam(name: 'EU', defaultValue: false),
                      booleanParam(name: 'US', defaultValue: false)
                    ])

                def stagingEnvs =
                    // Take the String value between
                    // the [ and ] brackets.
                    env.DEPLOY_STAGING
                        // Split on , to get a List.
//                         .split(', ')
//                         // Each list item is transformed
//                         // to a Map entry with key/value.
//                         .collectEntries { entry ->
//                             def pair = entry.split('=')
//                             [(pair.first()): pair.last()]
//                         }
                  
                  
                  }
                } catch (err) {
                  env.DEPLOY_STAGING = false
                }

              }
//               echo "Agreed to DEPLOY to Staging: ${stagingEnvs}"
              
            }
          }

          stage('Deploy Playground') {
            steps {
              input(message: 'Deploy to playground?', ok: 'EU, US, AP')
              waitUntil()
            }
          }

        }
      }

      stage('error') {
        agent {
          node {
            label 'backend'
          }

        }
        steps {
          echo 'Fininshed'
        }
      }

    }
  }
