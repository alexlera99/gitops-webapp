pipeline {
  agent any
  tools {
    go 'go'
    dockerTool 'docker'
  }
  stages {
    stage('Build') {
      steps {
        sh 'go build -o main main.go'
      }
    }

  }
}
