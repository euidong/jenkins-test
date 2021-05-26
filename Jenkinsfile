import groovy.json.JsonSlurper

def commitMessage = ""
def commitUrl = ""
def jobInfo = ""

@NonCPS
def runCommandToRemoteHosts(command) {
  def remoteHostsString = "${env.REMOTE_HOSTS}"
  def remoteHosts = ['test1', 'test2']
  
  remoteHosts.each { remoteHost ->
    sshPublisher(failOnError: true, publishers: [
      sshPublisherDesc(
        configName: "${remoteHost}",
        verbose: true,
        transfers: [
          sshTransfer(execCommand: "${command}")
        ]
      )
    ])
  }
}

pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        echo 'start build'
        script {
          commitMessage = sh(returnStdout: true, script: 'git log -1 --pretty=%B').trim()
          commitUrl = "${env.GIT_URL.substring(0, env.GIT_URL.indexOf('.git'))}/commit/${env.GIT_COMMIT}"
          jobInfo = "[${env.BUILD_NUMBER}]"
         
        }
        dir(path: 'externals/mine-collector') {
          sh 'make build TAG=test'
        }
      }
      post {
        success {
          slackSend(
            color: "good",
            message: "${jobInfo}\n✅ 빌드 성공\nCommit: ${commitMessage}\n${commitUrl}"
          )
        }
        failure {
          slackSend(
            color: "danger",
            message: "${jobInfo}\n❌ 빌드 실패\nCommit: ${commitMessage}\n${commitUrl}"
          )
        }
      }
    }

    stage('deploy') {
      script {
        steps {
          runCommandToRemoteHosts("ls -al")
        }
      }
      post {
        success {
          slackSend(
            color: "good",
            message: "${jobInfo}\n✅ 배포 성공\nCommit: ${commitMessage}\n${commitUrl}"
          )
        }
        failure {
          slackSend(
            color: "danger",
            message: "${jobInfo}\n❌ 배포 실패\nCommit: ${commitMessage}\n${commitUrl}"
          )
        }
      }
    }
  }
}
