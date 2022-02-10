// pipeline {
//     agent { docker { image 'busybox:latest' } }
//     triggers {
//         pollSCM '* * * * *' 
//     }
//     stages {
//         stage('Build') { 
//             steps {
//                 echo "Hello Shalini and Benly !!!"
//             }
//         }
// }

pipeline {
    agent { docker { image 'maven:3.8.4-openjdk-11-slim' } }
    triggers {
        pollSCM '* * * * *' 
    }    
    stages {
        stage('build') {
            steps {
                sh 'mvn --version'
            }
        }
    }
}