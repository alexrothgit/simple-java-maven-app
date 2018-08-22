pipeline {
    /* agent { docker { image 'alexroth/myjenkinsslave:latest' }  */
    agent { 
        docker { 
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        } 
    }
    
    environment {
        // some example vars
        DISABLE_AUTH = 'true'
        DB_ENGINE    = 'sqlite'
    }
    stages {
       stage('QA') {
            steps {
                sh 'mvn --version'
                sh ' printenv'
                sh 'mvn sonar:sonar   -Dsonar.organization=alexrothgit-github   -Dsonar.host.url=https://sonarcloud.io   -Dsonar.login=ae4d20e7653555e6d1329ab37c2ace712898fa70'
            }
       }    
       stage('build') {
            steps {
                sh 'mvn --version'
                sh ' printenv'
                sh 'mvn -B -DskipTests clean package'
            }
       }
        stage('Test') {
            steps {
                sh 'mvn test'
                sh 'echo "Success???"'
            }
            post {
                 always {
                          echo 'This will always run after stage TEST'
                          junit 'target/surefire-reports/*.xml'
                  }
            }
        }
        stage('Deliver') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
            }
        }  
    }
    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}


