pipeline {
   agent any
tools {
 jdk 'java-8'
 maven 'maven'
 }
stages {
stage("init"){
steps {
sh '''
echo "PATH = ${PATH}"
echo "M2_HOME = ${M2_HOME}"
        '''
     }
   
   }
    stage("build") {
        steps {
            sh 'mvn clean package checkstyle:checkstyle'
        }
          post {
              success{
                   echo "archive artifact"
                   archiveArtifacts '**/*.war/'
                   echo "junit report"
                   junit '**/target/surefire-reports/*.xml'
                   echo "code quality checksum"
                    checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
                }
            }     
       }       
       stage ("deploy"){
           steps {
                build 'dev-deployment'
           }
       stage ("delivery"){
           steps {
               timeout(2) 
               input 'do you want to deploy in production'
           }
       }
       }
    }
} 