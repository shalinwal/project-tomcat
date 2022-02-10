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
    agent { 
        any
    }
    triggers {
        pollSCM '* * * * *' 
    }    
    stages {
        stage('build') {
            steps {
                echo "Hello Shalini and Benly !!!"
            }
        }
    }
}