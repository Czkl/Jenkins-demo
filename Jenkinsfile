pipeline {
    agent any
    environment{
        image_version="v2"
        JENKINS_ACCOUNT = credentials('jenkins-account')
    }
    stages {
        stage('build'){
            steps{
                sh "echo starting build"
            }
        }
        stage('test') {
            steps {
                script {
                    sh './ci testPG'
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
