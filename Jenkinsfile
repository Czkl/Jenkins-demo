pipeline {
    agent any
    environment{
        image_version="v2"
        DOCKERHUB_ACCOUNT = credentials ('dockerhub_account')
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
                   echo $image_version
                   docker build -t my-jenkins-app:${BUILD_NUMBER} .
                '''
            }
        }
        stage('Push Docker'){
            agent{
                docker{
                    image 'docker'
                }
            }
            steps {
                sh '''
                    sudo echo "${DOCKERHUB_ACCOUNT_PSW}" | docker login -u ${DOCKERHUB_ACCOUNT_USR}  --password-stdin
                    sudo docker push {DOCKERHUB_ACCOUNT_USR}/my-jenkins-app:${BUILD_NUMBER}
                '''
            }
        }
    }
    post {
        success {
            echo 'This will run only if successful'
            sh '''
                docker stop my-jenkins-app || true
                docker rm my-jenkins-app || true 
                docker run -d --name my-jenkins-app -p 8081:8080 my-jenkins-app:$image_version
            '''
        }
    }
}
