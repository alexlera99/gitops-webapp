pipeline {
  environment { 
        registry = "alexlera99" 
        registryCredential = 'dockerhub_id' 
        dockerImage = '' 
    }
  agent any
  stages {
    stage('Run CI?') {
      agent any
      steps {
        script {
          if (sh(script: "git log -1 --pretty=%B | fgrep -ie '[skip ci]' -e '[ci skip]'", returnStatus: true) == 0) {
            currentBuild.result = 'NOT_BUILT'
            error 'Aborting because commit message contains [skip ci]'
          }
        }
      }
    }
    stage('Build') {
      steps {
        sh 'go build -o main main.go'
        script { 
            dockerImage = docker.build(registry + "/gitops-webapp:$GIT_COMMIT")
            docker.withRegistry( '', registryCredential ) { 
                dockerImage.push() 
            }
        } 
      }
    }
    stage('deploy-dev') {
      steps {       
        sshagent(['jenkinsid']) {
            sh 'git remote set-url origin git@github.com:alexlera99/gitops-webapp.git'
            sh 'git config --global user.email gitlab@gitlab.com'
            sh 'git config --global user.name alexlera99'
            sh 'git checkout -B master'
            sh script: '''
            #!/bin/bash
              cd deployment/dev
              kustomize edit set image alexlera/gitops-webapp:${GIT_COMMIT}
              cat kustomization.yaml
            '''
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
