pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        echo 'start build'
        dir(path: 'externals/mine-collector') {
          sh 'make build TAG=test'
        }
      }
      post {
        success {
          slackSend(color: '#00FF00', message: "빌드 성공 - [${env.BUILD_NUMBER}] ${env.JOB_NAME} (${env.BUILD_URL})")
        }
        failure {
          slackSend(color: '#FF0000', message: "빌드 실패 - [${env.BUILD_NUMBER}] ${env.JOB_NAME} (${env.BUILD_URL})")
        }
      }
    }

    stage('deploy') {
      steps {
        sshPublisher(failOnError: true, publishers: [
            sshPublisherDesc(
              configName: 'test',
              verbose: true,
              transfers: [
                sshTransfer(execCommand: "ls -al")
              ]
            )
        ])
      }
      post {
        success {
          slackSend(color: '#00FF00', message: "배포 성공 - [${env.BUILD_NUMBER}] ${env.JOB_NAME} (${env.BUILD_URL})")
        }
        failure {
          slackSend(color: '#FF0000', message: "배포 실패 - [${env.BUILD_NUMBER}] ${env.JOB_NAME} (${env.BUILD_URL})")
        }
      }
    }
  }
}
