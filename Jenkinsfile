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

// pipeline {
//     agent none
//     triggers {
//         pollSCM '* * * * *' 
//     }    
//     stages {
//         stage('build') {
//             steps {
//                 echo "Hello Shalini and Benly , good job !!!"
//             }
//         }
//     }
// }

pipeline {
    agent {
        node {
            label 'master'
    triggers {
        pollSCM '* * * * *' 
    }    
    stages {
        stage('build') {
            steps {
                echo "I am doing good"
            }
        }
    }
}