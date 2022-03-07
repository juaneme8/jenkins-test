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
        telegramSend(message: 'Hola soy el bot de Jenkins!', chatId: 552249970)
        echo 'telegramSend \'Hello World\''
      }
    }

  }
}