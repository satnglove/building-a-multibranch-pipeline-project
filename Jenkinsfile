pipeline {
    agent {
        docker {
            image 'node:8-alpine'
            args '-p 3000:3000 -p 5001:5000'
        }
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                //sh  'mkdir /.npm'
                //sh  'chown -R $USER:root /.npm'
                //sh  'chown -R $USER:root /.config'
                sh  'echo $USER'
                sh  'ls /'
                sh  'pwd'
                sh  'npm -v'
                sh  'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }

    }
}
