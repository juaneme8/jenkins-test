pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Build stage'
      }
    }

    stage('Telegram') {
      steps {
        telegramSend(message: 'Hola soy el bot de Jenkins!', chatId: -625245891)
        echo 'telegramSend \'Hello World\''
      }
    }

  }
}