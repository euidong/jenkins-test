pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        echo 'start build'
        sh 'cd externals/mine-collector'
        sh 'ls -al'
        sh 'make build TAG=test'  
      }
    }
  }
}
