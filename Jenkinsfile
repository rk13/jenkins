pipeline {
    //agent { label 'smith' }
    
    options {
        skipDefaultCheckout true
    }

    parameters {
        string(name: 'Greeting', defaultValue: 'Hello', description: 'How should I greet the world?')
        booleanParam(defaultValue: false, description: 'Perform rollback to previous system state (docker image tag)', name: 'performRollback')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        if ($ { params.performRollback }) {
            stage('Rollback') {
                steps {
                    sh 'echo "Performing rollback"'
                }
            }

        }
        stage('Build') {
            steps {
                retry(3) {
                    sh 'echo "Hello World"'
                    sh '''
                    echo "Multiline shell steps works too"
                    ls -lah
                '''
                }
            }
        }
        stage('Test') {
            steps {
                sh 'echo "Testing ..."'
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

