pipeline {
    agent any
    environment {
        LT_BUILD_NAME = "lambdatest-pipeline"
    }
    stages {
    stage('setup') {
        steps {
        // url lambdatest tunnel file
        sh 'curl -O https://downloads.lambdatest.com/tunnel/v3/linux/64bit/LT_Linux.zip'
        //sh 'sudo apt-get install zip unzip'
        sh 'unzip -o LT_Linux.zip'
        // We use Environmental variables for storing username and access key as this is the best practice
        // We also need to add these environmental variable in jenkins:
        // http://192.168.1.52:8080/manage/configure
        //      Global properties: Environment variables
        sh './LT --user ${LT_USERNAME} --key ${LT_ACCESS_KEY} --tunnelName jenkins-tunnel --infoAPIPort 8000 &'
        

        }
    }

    stage('test') {
        steps {
        // Testing
        sh 'sleep 5'
        sh 'python3 -m http.server 8081 &'
        sh 'python3 sample-selenium-app.py'
        sh 'pkill -f "http.server"'
        sh 'sleep 10'
        sh 'curl -X DELETE http://127.0.0.1:8000'
        }
    }

    }
}
