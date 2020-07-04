pipeline {
    agent any
    environment{
        image_version="v2"
        docker_account= credentials('account')
    }
    stages {
        stage('Build') {
            steps {
                script {
                    sh '''
                       v=`expr ${BUILD_NUMBER} - 1`
                       echo ${JOB_URL}
                       echo ${JOB_NAME}
                       echo ${JOB_BASE_NAME}
                       echo ${docker_account.username}
                       echo ${dcoker_account.password}
                       lastbuild=`curl -u admin:admin ${JOB_URL}/${v}/buildTimestamp`
                       today=`date +"%-m/%-d/%y %R %p"`
                       if ["${today%/*}" -gt "${lastbuild%/*}"]
                       then echo ${today} 
                       else echo ${lastbuild} 
                       fi
                    '''
                }
            }
        }
        stage('Test') {
            steps {
                sh 'pwd'
            }
        }
    }
    post {
        success {
            echo 'This will run only if successful'
        }
    }
}
