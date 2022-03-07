pipeline {
  agent any

environment {
    registry = "registry.cime.com.ar/jenkins-test"  
    tagName = "${env.BRANCH_NAME == "main" ? "latest" : "dev"}"  
    dockerFile = "${env.BRANCH_NAME == "main" ? "Dockerfile" : "Dockerfile.staging"}"  
    registryCredential = 'registry'
    dockerImage = ''
  }

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
                
                sh 'curl -s -X \
                POST https://api.telegram.org/bot${TOKEN}/sendMessage \
                -d chat_id=${CHAT_ID} \
                -d parse_mode="HTML" \
                -d text="🛠 **Jenkins CI:** Iniciando build $BUILD_DISPLAY_NAME $JOB_NAME"'
              }
            }
        }
    }

  }
}
