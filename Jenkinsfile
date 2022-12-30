/* groovylint-disable NestedBlockDepth */

import org.jenkinsci.plugins.pipeline.modeldefinition.Utils

pipeline {
    agent any
    stages {
        stage('Deploy Staging') {
            stages {
                stage('Choose partitions') {
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
                                                    booleanParam(name: 'US', defaultValue: false),
                                                    booleanParam(name: 'AP', defaultValue: false)
                                            ]
                                    )
                                }

                                stagingEnvs = getMapFromString(env.DEPLOY_STAGING)
                                env.DEPLOY_STAGING_EU = stagingEnvs.EU
                                env.DEPLOY_STAGING_US = stagingEnvs.US
                                env.DEPLOY_STAGING_AP = stagingEnvs.AP
                            } catch (err) {
                                env.DEPLOY_STAGING_EU = false
                                env.DEPLOY_STAGING_US = false
                                env.DEPLOY_STAGING_AP = false
                            }
                        }
                    }
                }

                stage('Deploy partitions in Parallel') {
                    parallel {
                        stage('Deploy EU') {
                            steps {
                                script {
                                    if (env.DEPLOY_STAGING_EU == 'true') {
                                        // runDeploymentScript('eu', 'staging', params.VER, 'true')
                                        echo 'Starting DEPLOY to Staging EU'
                                    } else {
                                        Utils.markStageSkippedForConditional('Deploy EU')
                                        echo 'DEPLOY to Staging EU not performed'
                                    }
                                }
                            }
                        }
                        stage('Deploy US') {
                            steps {
                                script {
                                    if (env.DEPLOY_STAGING_US == 'true') {
                                        // runDeploymentScript('us', 'staging', params.VER, 'true')
                                        echo 'Starting DEPLOY to Staging US'
                                    } else {
                                        Utils.markStageSkippedForConditional('Deploy US')
                                        echo 'DEPLOY to Staging US not performed'
                                    }
                                }
                            }
                        }
                        stage('Deploy AP') {
                            steps {
                                script {
                                    if (env.DEPLOY_STAGING_AP == 'true') {
                                        // runDeploymentScript('ap', 'staging', params.VER, 'true')
                                        echo 'Starting DEPLOY to Staging AP'
                                    } else {
                                        Utils.markStageSkippedForConditional('Deploy AP')
                                        echo 'DEPLOY to Staging AP not performed'
                                    }
                                }
                            }
                        }
                    }
                }
            }
        }
        stage('Deploy Playground') {
            stages {
                stage('Choose partitions') {
                    steps {
                        echo 'Playground'
                        script {
                            Map playgroundEnvs = null
                            try {
                                timeout(time: 60, unit: 'MINUTES') {
                                    env.DEPLOY_PLAYGROUND = input(
                                            message: 'Choose the partitions in Playground you want to deploy to',
                                            parameters: [
                                                    booleanParam(name: 'EU', defaultValue: false),
                                                    booleanParam(name: 'US', defaultValue: false),
                                                    booleanParam(name: 'AP', defaultValue: false)
                                            ]
                                    )
                                }

                                playgroundEnvs = getMapFromString(env.DEPLOY_PLAYGROUND)
                                env.DEPLOY_PLAYGROUND_EU = playgroundEnvs.EU
                                env.DEPLOY_PLAYGROUND_US = playgroundEnvs.US
                                env.DEPLOY_PLAYGROUND_AP = playgroundEnvs.AP
                            } catch (err) {
                                env.DEPLOY_PLAYGROUND_EU = false
                                env.DEPLOY_PLAYGROUND_US = false
                                env.DEPLOY_PLAYGROUND_AP = false
                            }
                        }
                    }
                }

                stage('Deploy partitions in Parallel') {
                    parallel {
                        stage('Deploy EU') {
                            steps {
                                // echo "Agreed to DEPLOY to Staging: ${stagingEnvs.EU}"

                                script {
                                    if (env.DEPLOY_PLAYGROUND_EU == 'true') {
                                        // runDeploymentScript('eu', 'playground', params.VER, 'false')
                                        echo 'Starting DEPLOY to playground EU'
                                    } else {
                                        Utils.markStageSkippedForConditional('Deploy EU')
                                        echo 'DEPLOY to playground EU not performed'
                                    }
                                }
                            }
                        }
                        stage('Deploy US') {
                            steps {
                                script {
                                    if (env.DEPLOY_PLAYGROUND_US == 'true') {
                                        // runDeploymentScript('us', 'playground', params.VER, 'false')
                                        echo 'Starting DEPLOY to playground US'
                                    } else {
                                        Utils.markStageSkippedForConditional('Deploy US')
                                        echo 'DEPLOY to playground US not performed'
                                    }
                                }
                            }
                        }
                        stage('Deploy AP') {
                            steps {
                                script {
                                    if (env.DEPLOY_PLAYGROUND_AP == 'true') {
                                        // runDeploymentScript('ap', 'playground', params.VER, 'false')
                                        echo 'Starting DEPLOY to playground AP'
                                    } else {
                                        Utils.markStageSkippedForConditional('Deploy AP')
                                        echo 'DEPLOY to playground AP not performed'
                                    }
                                }
                            }
                        }
                    }
                }
            }
        }
        stage('Deploy Production') {
            when {
                anyOf {
                    branch 'master'
                    tag '*.*.*'
                }
            }
            stages {
                stage('Choose partitions') {
                    steps {
                        echo 'Production'
                        script {
                            Map productionEnvs = null
                            try {
                                timeout(time: 60, unit: 'MINUTES') {
                                    env.DEPLOY_PRODUCTION = input(
                                            message: 'Choose the partitions in Production you want to deploy to',
                                            parameters: [
                                                    booleanParam(name: 'EU', defaultValue: false),
                                                    booleanParam(name: 'US', defaultValue: false),
                                                    booleanParam(name: 'AP', defaultValue: false)
                                            ]
                                    )
                                }

                                productionEnvs = getMapFromString(env.DEPLOY_PRODUCTION)
                                env.DEPLOY_PRODUCTION_EU = productionEnvs.EU
                                env.DEPLOY_PRODUCTION_US = productionEnvs.US
                                env.DEPLOY_PRODUCTION_AP = productionEnvs.AP
                            } catch (err) {
                                env.DEPLOY_PRODUCTION_EU = false
                                env.DEPLOY_PRODUCTION_US = false
                                env.DEPLOY_PRODUCTION_AP = false
                            }
                        }
                    }
                }

                stage('Deploy partitions in Parallel') {
                    parallel {
                        stage('Deploy EU') {
                            steps {
                                // echo "Agreed to DEPLOY to Staging: ${stagingEnvs.EU}"

                                script {
                                    if (env.DEPLOY_PRODUCTION_EU == 'true') {
                                        // runDeploymentScript('eu', 'production', params.VER, 'false')
                                        echo 'Starting DEPLOY to production EU'
                                    } else {
                                        Utils.markStageSkippedForConditional('Deploy EU')
                                        echo 'DEPLOY to production EU not performed'
                                    }
                                }
                            }
                        }
                        stage('Deploy US') {
                            steps {
                                script {
                                    if (env.DEPLOY_PRODUCTION_US == 'true') {
                                        // runDeploymentScript('us', 'production', params.VER, 'false')
                                        echo 'Starting DEPLOY to production US'
                                    } else {
                                        Utils.markStageSkippedForConditional('Deploy US')
                                        echo 'DEPLOY to production US not performed'
                                    }
                                }
                            }
                        }
                        stage('Deploy AP') {
                            steps {
                                script {
                                    if (env.DEPLOY_PRODUCTION_AP == 'true') {
                                        // runDeploymentScript('ap', 'production', params.VER, 'false')
                                        echo 'Starting DEPLOY to production AP'
                                    } else {
                                        Utils.markStageSkippedForConditional('Deploy AP')
                                        echo 'DEPLOY to production AP not performed'
                                    }
                                }
                            }
                        }
                    }
                }
            }
        }
    }
}

def getMapFromString(paramsString) {
    return paramsString[1..-2]
        .split(', ')
        .collectEntries { entry ->
            def pair = entry.split('=')
            [(pair.first()): pair.last()]
        }
}
