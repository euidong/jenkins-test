def message = ""
def commitUrl = ""

pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        echo 'start build'
        script {
          commitMessage = sh(returnStdout: true, script: 'git log -1 --pretty=%B').trim()
          commitUrl = "${env.GIT_URL}/commit/${env.GIT_COMMIT}"
        }
        dir(path: 'externals/mine-collector') {
          sh 'make build TAG=test'
        }
      }
      post {
        success {
          slackSend(
            color: "good",
            message: "빌드 성공\nCommit: ${message}\nurl: ${commitUrl}"
          )
        }
        failure {
          slackSend(
            color: "danger",
            message: "빌드 실패\nCommit: ${message}\nurl: ${commitUrl}"
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
            message: "배포 성공\nCommit: ${message}\nurl: ${commitUrl}"
          )
        }
        failure {
          slackSend(
            color: "danger",
            message: "배포 실패\nCommit: ${message}\nurl: ${commitUrl}"
          )
        }
      }
    }
  }
}
