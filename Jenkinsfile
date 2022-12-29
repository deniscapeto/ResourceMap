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

                  }
                } catch (err) {
                  env.DEPLOY_STAGING = false
                }
              }

              echo "Agreed to DEPLOY to Staging: ${env.DEPLOY_STAGING}"
              echo "Agreed to DEPLOY to Staging: ${evaluate(env.DEPLOY_STAGING).US}"
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