pipeline {
  agent any

  environment {
    dockerImage = ''
  }

  stages {
     stage('Testing') {

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
                
                sh 'curl -x 172.30.221.240:8080 -s -X \
                POST https://api.telegram.org/bot${TOKEN}/sendMessage \
                -d chat_id=${CHAT_ID} \
                -d parse_mode="HTML" \
                -d text="🚀  <b>Jenkins CI:</b> <b>Pushing Notification</b> about $JOB_NAME"'
              }
            }
        }
    }

  }
}
