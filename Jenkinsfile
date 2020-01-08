pipeline {
    agent any
    environment{
        image_version = "v2"
    }
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'maven:3-alpine'
                    args '-v /root/.m2:/root/.m2'
                }
            }
            steps {
                sh 'mvn -B -DskipTests clean package'
                sh 'pwd'
                sh 'ls ./target'
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'maven:3-alpine'
                    args '-v /root/.m2:/root/.m2'
                }
            }
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deploy') {
            agent{
                docker{
                    image 'docker'
                }
            }
            steps {
                sh '''
                   pwd
                   ls
                   which docker
                   cat Jenkinsfile
                   ls target
                '''
                sh 'docker --version'
                sh '''
                   echo $image_version
                   docker build -t my-jenkins-app:$image_version .
                '''
            }
        }
    }
    post {
        success {
            echo 'This will run only if successful'
            sh '''
                docker stop my-jenkins-app
                docker rm my-jenkins-app
                docker run -d --name my-jenkins-app my-jenkins-app:$image_version
            '''
        }
    }
}
