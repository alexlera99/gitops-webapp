pipeline {
  agent any
  stages {
    stage('Build Docker Image') {
      steps {
        sh 'go build -o main main.go'
      }
    }

  }
}
