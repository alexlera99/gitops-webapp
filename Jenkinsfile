pipeline {
  agent any
  environment {
    dockerImage = 'version 23sdsds3'
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
        sh 'git remote set-url origin https://github.com/TheKvothe/jenkins-test.git'
        sh 'git config --global user.email "gitlab@gitlab.com"'
        sh 'git config --global user.name "GitLab CI/CD"'
        sh 'git checkout -B master'
        sh 'ls'
        sh script: '''
            #!/bin/bash
            echo "test" > test.txt
            git add .
            git commit -am "commit from pipeline"
            git push origin master
        '''
        
      }
    }
  }
}
