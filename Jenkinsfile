pipeline {
    agent  { label 'master' }
    tools {
        maven 'Maven 3.6.0'
        jdk 'jdk8'
    }
  environment {
    VERSION="0.1"

    GROUP = "neotysdevopsdemowithvirtualize"
    APP_NAME="ParasoftVirtualize"

  }
  stages {
      stage('Checkout') {
          agent { label 'master' }
          steps {
             println InetAddress.localHost.canonicalHostName
              git  url:"https://github.com/${GROUP}/${APP_NAME}.git",
                      branch :'master'
          }
      }
    stage('Docker build') {
        steps {
            withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'TOKEN', usernameVariable: 'USER')]) {

                sh "docker build -t neotysdevopsdemo/server-jre8 $WORKSPACE/server-jre8/"
                sh "docker build -t neotysdevopsdemo/tomcat8 $WORKSPACE/tomcat8/"
                sh "docker build -t neotysdevopsdemo/datarepository $WORKSPACE/datarepository/"
                sh "docker build -t neotysdevopsdemo/soavirt $WORKSPACE/soavirt/"
                sh "docker build -t neotysdevopsdemo/ctp $WORKSPACE/ctp/"
                sh "docker login --username=${USER} --password=${TOKEN}"
                sh "docker push neotysdevopsdemo/soavirt"
                sh "docker push neotysdevopsdemo/ctp"
            }
        }
     }
     stage('create parasoft netwrok') {

             steps {
                  sh "docker network create parasoft || true"

             }
     }
     stage('deploy CTP') {

                steps {
                     sh "docker-compose -f  ./ctp/docker-compose.yml up"

                }
     }
     stage('deploy soavir') {

                 steps {
                      sh "docker-compose -f  ./soavirt/docker-compose.yml up"

                 }
      }
  }
}