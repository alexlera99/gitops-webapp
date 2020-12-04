pipeline {
  agent any
  environment {
    dockerImage = 'version 233'
  }
  tools {
    go 'go'
  }
  stages {

    stage('Build') {
      steps {
        sh 'go build -o main main.go'
        sh 'docker build -t gitops-webapp:${GIT_COMMIT} .'
      }
    }
    stage('deploy-dev'){
      steps{
        sh 'git remote set-url origin https://github.com/TheKvothe/gitops-webapp.git'
        sh 'git config --global user.email "gitlab@gitlab.com"'
        sh 'git config --global user.name "GitLab CI/CD"'
      }
    }
  }
}
