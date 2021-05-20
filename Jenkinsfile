pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        echo 'start build'
        sh 'apt-get install build-essential'
        dir('externals/mine-collector') {
          sh 'make build TAG=test'
        }  
      }
    }
  }
}
