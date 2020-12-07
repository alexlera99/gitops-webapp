pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'go build -o main main.go'
        sh 'docker build -t gitops-webapp:${GIT_COMMIT} .'
      }
    }

    stage('deploy-dev') {
      steps {
        sh '''
            #!/bin/bash
            git remote set-url origin https://github.com/alexlera99/jenkins-test.git
            git config --global user.email "gitlab@gitlab.com"
            git config --global user.name "GitLab CI/CD"
            git checkout -B master
            echo "test" > test.txt
            git add .
            git commit -am "commit from pipeline"
            git push origin master
        '''
      }
    }

  }
  tools {
    go 'go'
    dockerTool 'docker'
  }
}
