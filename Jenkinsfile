pipeline {
  agent any
  stages {
    stage('build') {
      steps {
        echo 'start build'
        sh 'cat /etc/issue'
        sh 'apk add make'
        dir('externals/mine-collector') {
          sh 'make build TAG=test'
        }  
      }
    }
  }
}
