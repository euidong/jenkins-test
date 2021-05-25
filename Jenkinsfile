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
          slackSend(
            color: "good",
            message: "빌드 성공\nCommit: [${env.GIT_COMMIT}]${env.GIT_URL}"
          )
        }
        failure {
          slackSend(
            color: "danger",
            message: "빌드 실패\nCommit: [${env.GIT_COMMIT}]${env.GIT_URL}"
          )
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
          slackSend(
            color: "good",
            message: "배포 성공\nCommit: [${env.GIT_COMMIT}]${env.GIT_URL}"
          )
        }
        failure {
          slackSend(
            color: "good",
            message: "배포 실패\nCommit: [${env.GIT_COMMIT}]${env.GIT_URL}"
          )
        }
      }
    }
  }
}
