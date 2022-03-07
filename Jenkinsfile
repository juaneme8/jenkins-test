pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Build stage'
      }
    }

    stage('Push Notification') {
        steps {
            script{
              
              withCredentials([string(credentialsId: 'telegramToken', variable: 'TOKEN'),
              string(credentialsId: 'telegramChatId', variable: 'CHAT_ID')]) {
                
                sh 'curl -s -X POST https://api.telegram.org/bot${TOKEN}/sendMessage -d chat_id=${CHAT_ID} -d parse_mode="HTML" -d text="<b>Project</b> : Jenkins Test%0A \
                <b>🌿 Branch</b>: main%0A \
                <b>🚀 Build </b> : OK!%0A \
                <b>🆗 Test suite</b> = Passed!"'
              }
            }
        }
    }

  }
}
