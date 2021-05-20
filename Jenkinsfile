pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        echo 'start build'
        sh 'docker ps'
        sh 'apk add make'
        dir('externals/mine-collector') {
          sh 'make build TAG=test'
        }  
      }
    }
  }
}
