def commitMessage = ""
def commitUrl = ""
def jobInfo = ""

pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        echo 'start build'
        script {
          commitMessage = sh(returnStdout: true, script: 'git log -1 --pretty=%B').trim()
          commitUrl = "${env.GIT_URL.substring(0, env.GIT_URL.indexOf('.git'))}/commit/${env.GIT_COMMIT}"
          jobInfo = "[${env.BUILD_NUMBER}-${env.JOB_NAME}]]"
        }
        dir(path: 'externals/mine-collector') {
          sh 'make build TAG=test'
        }
      }
      post {
        success {
          slackSend(
            color: "good",
            message: "${jobInfo}빌드 성공\nCommit: ${commitMessage}\nurl: ${commitUrl}"
          )
        }
        failure {
          slackSend(
            color: "danger",
            message: "${jobInfo}빌드 실패\nCommit: ${commitMessage}\nurl: ${commitUrl}"
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
            message: "${jobInfo}배포 성공\nCommit: ${commitMessage}\nurl: ${commitUrl}"
          )
        }
        failure {
          slackSend(
            color: "danger",
            message: "${jobInfo}배포 실패\nCommit: ${commitMessage}\nurl: ${commitUrl}"
          )
        }
      }
    }
  }
}
