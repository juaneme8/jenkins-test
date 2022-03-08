pipeline {
  agent any

  environment {
    dockerImage = ''
  }

  stages {

     stage('Testing') {
      agent {
        docker {
          image 'node:10'
        }

      }
      steps {
        echo 'testing'
      }
    }

   stage('Build image') {
      steps {
        script{
          dockerImage = docker.build("juaneme8/hellonode")
        }
      }
    }

    stage('Push image') {
      agent {
        node {
          label 'master'
        }
      }  
      
      when { 
        beforeAgent true
        anyOf {
          branch 'main'
          branch 'staging' 
        }
      }

      steps{
        script {
          docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
            dockerImage.push("${env.BUILD_NUMBER}")
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Push Notification') {
        steps {
            script{
              
              withCredentials([string(credentialsId: 'telegramToken', variable: 'TOKEN'),
              string(credentialsId: 'telegramChatId', variable: 'CHAT_ID')]) {
                
                sh 'curl -s -X \
                POST https://api.telegram.org/bot${TOKEN}/sendMessage \
                -d chat_id=${CHAT_ID} \
                -d parse_mode="HTML" \
                -d text="🚀  <b>Jenkins CI:</b> <b>Iniciando build $BUILD_DISPLAY_NAME</b> $JOB_NAME"'
              }
            }
        }
    }

  }
}
