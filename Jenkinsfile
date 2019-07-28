pipeline {
    agent { label 'smith' }
    options {
        skipDefaultCheckout true
    }

    parameters {
        booleanParam(defaultValue: true, description: '', name: 'userFlag')
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
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

