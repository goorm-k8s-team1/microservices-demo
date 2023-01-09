pipeline{
  agent any

  environment{
    HOME = '.'
  }

  options {
    disableConcurrentBuilds()
  }

  stages {
      // 레포지토리를 다운로드 받음
      stage('Prepare') {
        agent any
        steps {
            echo 'Clonning Repository'

            checkout scm
        }
      }
      stage('Build&Push Docker Image') {
        agent any

        steps {
          echo 'Build&Push Image'

          script {
            def folders = []
            sh(returnStdout: true, script: "ls ./src").split().each {
              folders << it
            }
            folders.each { item ->
              if (item == "cartservice") {
                def app = docker.build("wndudwns0028/${item}", "./src/${item}/src")
                docker.withRegistry('https://registry.hub.docker.com', 'docker-credential') {
                  app.push("${env.BUILD_NUMBER}")
                  app.push("latest")
                }
              } else {
                def app = docker.build("wndudwns0028/${item}", "./src/${item}")
                docker.withRegistry('https://registry.hub.docker.com', 'docker-credential') {
                  app.push("${env.BUILD_NUMBER}")
                  app.push("latest")
                }
              }
            }
          }
        }
      }
  }
}

