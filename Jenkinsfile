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
                    def lastBuildTime = sh(script: "curl -u ${JENKINS_ACCOUNT_USR}:${JENKINS_ACCOUNT_PSW} ${JOB_URL}/lastBuild/buildTimestamp", returnStdout: true).trim()
                    def today = sh(script: "date +'%-m/%-d/%y %-R %p'", returnStdout: true).trim()
                    def failure = sh(script: "curl -u ${JENKINS_ACCOUNT_USR}:${JENKINS_ACCOUNT_PSW} ${JOB_URL}/lastBuild/api/xml | grep -c '<result>FAILURE</result>' || true", returnStdout: true).trim()
                    lastBuildTime = lastBuildTime.substring(0,lastBuildTime.lastIndexOf("/"))
                    today = today.substring(0,today.lastIndexOf("/"))
                    if (today.compareTo(lastBuildTime) || failure == "1") {
                        sh
                    }

                }
                steps {
                    sh ' echo "test pg"'
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
