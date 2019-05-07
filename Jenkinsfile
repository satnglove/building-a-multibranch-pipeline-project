pipeline {
    agent {
        docker {
            image 'node:6-alpine'
            args '-p 3000:3000 -p 5000:5000'
        }
    }
    environment {
        CI = 'true'
        HOME="."
    }
    stages {

      stage('Check for review') {
            steps {
                script {
                    withCredentials([[$class: "UsernamePasswordMultiBinding", credentialsId: "47abe382-eb8b-42ce-9427-6197346f66ef", usernameVariable: "GHUSR", passwordVariable: "GHPWD"]]){
                        try {
                            GHURL = "https://api.github.com/repos/satnglove/building-a-multibranch-pipeline-project/pulls/${CHANGE_ID}"
                            sh "curl -u ${GHUSR}:${GHPWD} ${GHURL} 2>/dev/null |grep 'state'|grep -c 'APPROVED'"
                        } catch(error) {
                            echo "=================> Not approved, rejecting! <================="
                            throw error
                        }
                    }
                }
            }
        }

        stage('Build') {
            steps {
                //sh "npm config set prefix '/home/node'"
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deliver for development') {
            when {
                branch 'development'
            }
            steps {
                sh './jenkins/scripts/deliver-for-development.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
        stage('Deploy for production') {
            when {
                branch 'production'
            }
            steps {
                sh './jenkins/scripts/deploy-for-production.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }

}
