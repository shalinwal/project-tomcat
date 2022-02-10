pipeline {
    agent { docker { image 'busybox:latest' } }
    triggers {
        pollSCM '* * * * *' 
    }
    stages {
        stage('Build') { 
            steps {
                echo "Hello Shalini and Benly !!!"
            }
        }
}