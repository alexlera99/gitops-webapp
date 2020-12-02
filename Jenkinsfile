pipeline {
  agent any
  environment {
    dockerImage = ''
  }
  tools {
    go 'go'
  }
  stages {
    stage('Build') {
      steps {
        sh 'go build -o main main.go'
         dockerImage = docker.build registry + ":$BUILD_NUMBER" 
      }
    }

  }
}
