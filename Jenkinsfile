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
            blocks: [
              [
                type: "section",
                text: [
                  type: "mrkdwn",
                  text: "Commit - [${env.GIT_COMMIT}]${env.GIT_URL}"
                ]
              ],
              [
                type: "driver"
              ]
            ],
            attachments: [
              [
                text: 'Commit',
                fallback: '[${env.GIT_COMMIT}]${env.GIT_URL}',
                color: '#ff0000'
              ]
            ],
            message: "빌드 성공"
          )
        }
        failure {
          slackSend(
            color: "danger",
            message: "빌드 실패"
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
            message: "배포 성공"
          )
        }
        failure {
          slackSend(
            color: "good",
            message: "배포 성공"
          )
        }
      }
    }
  }
}
