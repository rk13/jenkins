#!groovy
def projectProperties = [[
                                 $class  : 'BuildDiscarderProperty',
                                 strategy: [
                                         $class               : 'LogRotator',
                                         numToKeepStr         : '8',
                                         daysToKeepStr        : '15',
                                         artifactNumToKeepStr : '8',
                                         artifactDaysToKeepStr: '15'
                                 ]
                         ],
                         parameters(
                                 [booleanParam(name: 'replaceInstances', defaultValue: false,
                                         description: 'Replace existing instances',),

                                  booleanParam(name: 'B2B_FAST_DEPLOY_FLAG', defaultValue: false,
                                          description: 'Perform fast deployment of specified previous system build (docker image tag)'),
                                  booleanParam(name: 'B2B_FAST_DEPLOY_LIVE', defaultValue: false,
                                          description: 'Perform fast deployment to Live system'),
                                  string(name: 'B2B_FAST_DEPLOY_VERSION', defaultValue: 'UNKNOWN',
                                          description: 'Previous system build (docker image tag)')]
                         )

]
properties(projectProperties)

try {
    // Init on master
    node {
        // The first milestone step starts tracking concurrent build order
//         milestone()
//         notifyBitbucketOfStatus()
    }
//    podTemplate(label: 'kubernetes-tools-extra-large', cloud: 'kubernetes-tools') {
//        node('kubernetes-tools-extra-large') {
    node() {
        stage('Setup') {
            checkout scm
//                    unlockGitCryptVault()
        }

        if (fullBuildRequired()) {
            stage('Build') {
                echo 'Performing full build'
//                        build()
//                        updateDisplayName()
            }
            stage('Publish to staging') {
                echo 'Performing publishing to staging'
//                        publishToStaging()
            }
        }

        if (fastBuildRequired()) {
            stage('Check fast build parameters') {
                echo 'Performing fast build only'
                checkFastBuildParams()
            }
        }

        if (onMaster()) {
            stage('Deploy all products to staging') {
                echo 'Deploy all products to staging'
            }
            stage('Publish to production') {
                echo 'Deploy all products to production'
            }
        }
    }
    currentBuild.result = 'SUCCESS'
} catch (error) {
    currentBuild.result = 'FAILED'
    throw error
}


private boolean onMaster() {
    env.BRANCH_NAME == 'master'
}

private boolean fullBuildRequired() {
    !params.B2B_FAST_DEPLOY_FLAG
}

private boolean fastBuildRequired() {
    params.B2B_FAST_DEPLOY_FLAG
}

private void checkFastBuildParams() {
    echo "Checking fast build parameters"
    echo "B2B_FAST_DEPLOY_FLAG: ${params.B2B_FAST_DEPLOY_FLAG}"
    echo "B2B_FAST_DEPLOY_LIVE: ${params.B2B_FAST_DEPLOY_LIVE}"
    echo "B2B_FAST_DEPLOY_VERSION: ${params.B2B_FAST_DEPLOY_VERSION}"

    if (params.B2B_FAST_DEPLOY_VERSION == null && params.B2B_FAST_DEPLOY_VERSION == 'UNKNOWN') {
        throw new Exception("B2B_FAST_DEPLOY_VERSION could not be empty for fast-deploy configuragion")
    }
}


