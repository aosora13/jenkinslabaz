pipeline {
    agent any
    // Uncomment the following block if you want to use a Docker agent
    // agent {
    //     docker {
    //         image 'docker:latest'
    //     }
    // }
    
    environment {
        dockerHome = tool 'myDocker'
        mavenHome = tool 'myMaven'
        PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
        
    }
    
    stages {
        stage('Checkout') {
            steps {
                sh 'mvn --version'
                sh 'docker version'
                echo "Build"
                echo "PATH - $PATH"
                echo "BUILD_NUMBER - $env.BUILD_NUMBER"
                echo "BUILD_ID - $env.BUILD_ID"
                echo "JOB_NAME - $env.JOB_NAME"
                echo "BUILD_TAG - $env.BUILD_TAG"
                echo "BUILD_URL - $env.BUILD_URL"
            }
        }
        
        stage('Compile') {
            steps {
                sh "mvn clean compile"
            }
        }
        
        stage('Test') {
            steps {
                sh "mvn test"
            }
        }
        
        stage('Integration Test') {
            steps {
                sh "mvn failsafe:integration-test failsafe:verify"
            }
        }
        
        stage('Package') {
            steps {
                sh "mvn package -DskipTests"
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Corrected docker build syntax
                    sh 'export DOCKER_HOST=tcp://172.21.58.55:2376'
                    dockerImage = docker.build("aosora13/currency-exchange-devops:$env.BUILD_TAG", ".")
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    // Corrected docker push syntax
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
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