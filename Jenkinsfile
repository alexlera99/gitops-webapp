pipeline {
  agent any
  environment {
    dockerImage = 'version 233'
  }
  tools {
    go 'go'
  }
  stages {
    
    stage('Checkout') {
      steps {
        scmSkip(deleteBuild: true, skipPattern:'.*\\[ci skip\\].*')
      }
    }

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
        sh 'git checkout -B master'
        sh 'cd deployment/dev'
        sh 'kustomize edit set image  gitops-webapp:${GIT_COMMIT} '
        sh 'cat kustomization.yaml'
        sh 'git commit -am "[skip ci] DEV image update"'
        sh 'git push origin master'
      }
    }
  }
}
