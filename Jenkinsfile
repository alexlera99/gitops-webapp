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
      sh 'apk add --no-cache git curl bash'
      sh 'curl -s "https://raw.githubusercontent.com/kubernetes-sigs/kustomize/master/hack/install_kustomize.sh"  | bash'
      sh 'mv kustomize /usr/local/bin/'
    }
  }
}
