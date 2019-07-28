pipeline {
    //agent { label 'smith' }

    options {
        skipDefaultCheckout true
    }

    parameters {
        string(name: 'Greeting', defaultValue: 'Hello', description: 'How should I greet the world?')

        choice(name: 'b2b_pipeline_action',
                defaultValue: 'build-and-deploy',
                description: 'Perform full build pipeline (default) or fast deployment of previous system build (docker image tag)',
                choices: ['build-and-deploy', 'fast-deploy']
                )
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            when {
                expression { return params.b2b_pipeline_action == 'build-and-deploy' }
            }
            steps {
                sh 'Building the system ..."'
            }
        }
        stage('Test') {
            when {
                expression { return params.b2b_pipeline_action == 'build-and-deploy' }
            }
            steps {
                sh 'echo "Testing the system ..."'
            }
        }
        stage('Check fast-deploy parameters') {
            when {
                expression {
                    return params.b2b_pipeline_action == 'fast-deploy'
                }
            }
            steps {
                sh 'echo "Check fast-deploy params"'
            }
        }
        stage('Deploy') {
            when {
                expression {
                    return params.b2b_pipeline_action == 'build-and-deploy' || params.b2b_pipeline_action == 'fast-deploy'
                }
            }
            steps {
                sh 'echo "Deploying the system ..."'
            }
        }
    }
    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}

