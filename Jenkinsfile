pipeline {
    agent {
        docker {
            image 'node:23.10.0-alpine'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'node --version'
                echo "Build"
            }
        }
        stage('Test') {
            steps {
                echo "Test"
            }
        }
        stage('Integration Test') {
            steps {
                echo "Integration Test"
            }
        }
    }
    post {
        always {
            echo "I am awesome. I run always"
        }
        success {
            echo "I run when you are successful"
        }
        failure {
            echo "I run when you fail"
        }
    }
}