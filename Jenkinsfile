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
                sh 'curl -s -X POST https://api.telegram.org/bot${TOKEN}/sendMessage -d chat_id=${CHAT_ID} -d parse_mode="HTML" -d text="<b>Project</b> : POC \
                <b>🌿 Branch</b>: <div style="color:red">main</div><br> \
                <b>🚀 Build </b> : OK \
                <b>🆗 Test suite</b> = Passed"'
              }
            }
        }
    }

  }
}
