pipeline {
    agent { docker { image 'maven:3.3.3' } }
    stages {
        stage('build') {
            steps {
                sh 'mvn --version'
                sh 'echo "Hello World"'
                sh '''
                     echo "Multiline shell steps works too"
                     ls -lah
                   '''
                   }
                        }
        stage('Deploy') {
            steps {         
                retry(3) {
                    sh './flakey-deploy.sh'
                         }
                timeout(time: 3, unit: 'MINUTES') {
                    sh './health-check.sh'
                                                   }
                  }        
                        } 
             }
}


