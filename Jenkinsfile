pipeline {
    agent any
    stages {
        stage('Deploy') {
            parallel {
                stage('Deploy Staging') {
                    steps {
                        echo 'Staging'
                        script {
                            Map stagingEnvs = null
                            try {
                                timeout(time: 60, unit: 'MINUTES') {
                                    env.DEPLOY_STAGING = input(
                                        message: 'Continue with Deploy to Staging EU?',
                                        parameters: [
                                            booleanParam(name: 'EU', defaultValue: false),
                                            booleanParam(name: 'US', defaultValue: false)
                                        ]
                                    )
                                }

                                stagingEnvs = env.DEPLOY_STAGING[1..-2]
                                .split(', ')
                                .collectEntries { entry ->
                                    def pair = entry.split('=')
                                    [(pair.first()): pair.last()]
                                }
                            } catch (err) {
                                env.DEPLOY_STAGING = false
                            }
                            println "Agreed to DEPLOY to Staging: ${stagingEnvs.EU}"
                        }
                        // echo "Agreed to DEPLOY to Staging: ${stagingEnvs.EU}"
                    }
                }

                stage('Deploy Playground') {
                    steps {
                        //input(message: 'Deploy to playground?', ok: 'EU, US, AP')
                        echo 'Deploy to playground'
                    }
                }
            }
        }

        stage('Final') {
            steps {
                echo 'Fininshed'
            }
        }
    }
}
