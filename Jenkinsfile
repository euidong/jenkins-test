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
                title: "Build",
              ],
              [
                title: "Commit",
                value: "[${env.GIT_COMMIT}]${env.GIT_URL}",
                short: true
              ]
            ],
            message: "빌드 성공"
          )
        }
        failure {
          slackSend(
            color: "danger",
            blocks: [
              [
                title: "Branch",
                value: "${env.GIT_BRANCH}",
                short: true
              ]
            ],
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
            blocks: [
              [
                title: "Build",
              ],
              [
                title: "Commit",
                value: "[${env.GIT_COMMIT}]${env.GIT_URL}",
                short: true
              ]
            ],
            message: "배포 성공"
          )
        }
        failure {
          slackSend(
            color: "good",
            blocks: [
              [
                title: "Build",
              ],
              [
                title: "Commit",
                value: "[${env.GIT_COMMIT}]${env.GIT_URL}",
                short: true
              ]
            ],
            message: "배포 성공"
          )
        }
      }
    }
  }
}
