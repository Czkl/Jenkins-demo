pipeline {
    agent any
    environment{
        image_version="v2"
    }
    stages {
        stage('Build') {
            agent {
                label 'master'
            }
            steps {
                sh 'pwd'
            }
        }
        stage('Test') {
            agent {
                label 'master'
            }
            steps {
                sh 'pwd'
            }
            post {

            }
        }
    }
}
