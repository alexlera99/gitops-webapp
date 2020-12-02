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
        sh 'docker build -t gitops-webapp:${GIT_COMMIT} .'
      }
    }
    stage('deploy-dev'){
      steps{
        sh 'curl -s "https://raw.githubusercontent.com/kubernetes-sigs/kustomize/master/hack/install_kustomize.sh"  | bash'
        sh 'git config --global user.email "gitlab@gitlab.com"'
        sh 'git config --global user.name "GitLab CI/CD"'
      }
    }
  }
}
