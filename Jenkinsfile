pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        echo 'start build'
        sh 'docker login'
        dir('externals/mine-collector') {
          sh 'make build TAG=test'
        }  
      }
    }
  }
}
