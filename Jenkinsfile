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
    }

    stage('deploy') {
      steps {
        sshPublisher(failOnError: true, publishers: [
            sshPublisherDesc(
              configName: "test-deploy",
              verbose: true,
              transpers: [
                sshTransfer(execCommand: "ls -al")
              ]
            )
        ], masterNodeName: 'test')
      }
    }
  }
}
