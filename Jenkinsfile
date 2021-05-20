pipeline {
  agent none
  stages {
    stage('build') {
			agent { 
				dockerfile true
			}
      steps {
        echo 'start build'
        dir('externals/mine-collector') {
          sh 'make build TAG=test'
        }  
      }
    }
  }
}
