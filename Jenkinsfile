pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Build stage'
      }
    }

    stage('') {
      steps {
        telegramSend(message: 'Hola soy el bot de Jenkins!', chatId: 552249970)
      }
    }

  }
}