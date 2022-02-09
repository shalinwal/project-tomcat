pipeline {
    agent { docker { image 'busybox:latest' } }
    triggers {
        pollSCM '* * * * *' 
    }
    stages {
        stage('Build') { 
            steps {
                println ("Hello Shalini and Benly !!!") 
            }
        }
}