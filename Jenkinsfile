pipeline {
  def app
  agent node

environment {
    registry = "registry.cime.com.ar/jenkins-test"  
    tagName = "${env.BRANCH_NAME == "main" ? "latest" : "dev"}"  
    dockerFile = "${env.BRANCH_NAME == "main" ? "Dockerfile" : "Dockerfile.staging"}"  
    registryCredential = 'registry'
    dockerImage = ''
  }

  stages {
   stage('Build image') {
        /* This builds the actual image; synonymous to docker build on the command line */

        app = docker.build("juaneme8/hellonode")
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:First, the incremental build number from Jenkins Second, the 'latest' tag.Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://registry.cime.com.ar', 'dockerhub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
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
