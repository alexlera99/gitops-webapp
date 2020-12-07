pipeline {
  agent any
  stages {
    stage('Checkout') {
         steps {
            scmSkip(deleteBuild: false, skipPattern:'.*\\[ci skip\\].*')
         }
    }
    stage('Build') {
      steps {
        sh 'go build -o main main.go'
        sh 'docker build -t gitops-webapp:${GIT_COMMIT} .'
      }
    }
    stage('deploy-dev') {
      steps {       
        sshagent(['jenkinsid']) {
            sh 'git remote set-url origin git@github.com:alexlera99/gitops-webapp.git'
            sh 'git config --global user.email gitlab@gitlab.com'
            sh 'git config --global user.name alexlera99'
            sh 'git checkout -B master'
            sh 'cd deployment/dev'
            sh 'kustomize edit set image gitops-webapp:${GIT_COMMIT}'
            sh 'cat kustomization.yaml'
            sh 'git commit -am "[ci skip] DEV image update"'
            sh 'git push origin master' 
        }
      }
    }

  }
  tools {
    go 'go'
    dockerTool 'docker'
    git 'git'
  }
}
