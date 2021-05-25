pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        echo 'start build'
        dir('externals/mine-collector') {
          sh 'make build TAG=test'
        }  
      }
    }
    stage('deploy') {
      steps([$class: 'BapSshPromotionPublisherPlugin']) {
        sshPublisher(
          continueOnError: false, failOnError: true,
          publishers: [
            sshPublisherDesc(
              execCommand: "ls -al"
            )
          ]
        )
      }
    }
  }
}
