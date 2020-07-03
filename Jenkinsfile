pipeline {
    agent any
    environment{
        image_version="v2"
    }
    stages {
        stage('Build') {
            steps {
                sh 'pwd'
            }
        }
        stage('Test') {
            steps {
                sh 'pwd'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
    }
    post {
        success {
            echo 'This will run only if successful'
        }
    }
}
