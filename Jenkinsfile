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
  }
}
