pipeline {
  agent any
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
    stage('deploy-dev') {
      steps {       
        sshagent(['jenkinsid']) {
            sh 'git remote set-url origin git@github.com:alexlera99/gitops-webapp.git'
            sh 'git config --global user.email gitlab@gitlab.com'
            sh 'git config --global user.name alexlera99'
            sh 'git checkout -B master'
            sh 'echo "test4" > test.txt'
            sh 'git add .'
            sh 'git commit -am "[ci skip] commit from pipeline"'
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
