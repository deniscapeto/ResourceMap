pipeline {
  agent any
  stages {
    stage('Deploy') {
      parallel {
        stage('Deploy Staging') {
          steps {
            echo 'Staging'
            input(message: 'deploy to staging', ok: 'EU, US, AP')
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

    stage('') {
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