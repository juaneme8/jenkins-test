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
          dockerImage = docker.build("registry.cime.com.ar/hellonode")
        }
      }
    }

    stage('Push image') {

      steps{
        script {
          docker.withRegistry('https://registry.cime.com.ar', 'registryCredentials') {
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
                
                sh '''
                curl -x 172.30.221.240:8080 -s -X \
                POST https://api.telegram.org/bot${TOKEN}/sendMessage \
                -d chat_id=${CHAT_ID} \
                -d parse_mode="HTML" \
                -d text="🚀  <b>Jenkins CI:</b> <b>Iniciando build $BUILD_DISPLAY_NAME</b> $JOB_NAME"
                '''
              }
            }
        }
    }

  }
}
