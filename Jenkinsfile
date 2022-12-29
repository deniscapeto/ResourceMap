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
                                env.DEPLOY_STAGING_EU = stagingEnvs.EU
                                env.DEPLOY_STAGING_US = stagingEnvs.US
                                env.DEPLOY_STAGING_AP = stagingEnvs.AP
                                
                            } catch (err) {
                                env.DEPLOY_STAGING_EU = false
                                env.DEPLOY_STAGING_US = false
                                env.DEPLOY_STAGING_AP = false
                            }
                        }
                        // echo "Agreed to DEPLOY to Staging: ${stagingEnvs.EU}"
                        parallel {

                            stage('Staging EU') {
                                steps {
                                    script {
                                        if (env.DEPLOY_STAGING_EU == 'true') {
                                            // runDeploymentScript('eu', 'staging', params.VER, 'true')
                                            echo "Starting DEPLOY to Staging EU"
                                        } else {
                                            // Utils.markStageSkippedForConditional("Staging EU")
                                            echo "DEPLOY to Staging EU not performed"
                                        }
                                    }
                                }
                            }
                            
                            
                            stage('Staging US') {
                                steps {
                                    script {
                                        if (env.DEPLOY_STAGING_US == 'true') {
                                            // runDeploymentScript('us', 'staging', params.VER, 'true')
                                            echo "Starting DEPLOY to Staging US"
                                        } else {
                                            // Utils.markStageSkippedForConditional("Staging US")
                                            echo "DEPLOY to Staging US not performed"
                                        }
                                    }
                                }
                            }
                            
                            
                            stage('Staging AP') {
                                steps {
                                    script {
                                        if (env.DEPLOY_STAGING_AP == 'true') {
                                            // runDeploymentScript('ap', 'staging', params.VER, 'true')
                                            echo "Starting DEPLOY to Staging AP"
                                        } else {
                                            // Utils.markStageSkippedForConditional("Staging AP")
                                            echo "DEPLOY to Staging AP not performed"
                                        }
                                    }
                                }
                            }
                                    
                                    
                        }
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
