pipeline {
    agent {
      kubernetes  {
            label 'jenkins-slave'
        }
    }
  environment {
      IMAGE_REPO = "teamcloudyuga/rsvp-demo"
      // Instead of teamcloudyuga, use your repo name
  }
  stages {
 
    stage('Build') {
      environment {
        DOCKERHUB_CREDS = credentials('dockerhub')
 
      }
      steps {
        container('docker') {
          // Build new image
          sh "until docker ps; do sleep 3; done && docker build -t  ${env.IMAGE_REPO}:${env.GIT_COMMIT} ."
          // Publish new image
          sh "docker login --username $DOCKERHUB_CREDS_USR --password $DOCKERHUB_CREDS_PSW && docker push ${env.IMAGE_REPO}:${env.GIT_COMMIT}"
        }
      }
    }
 
    stage('Deploy staging') {
      environment {
        GIT_CREDS = credentials('github')
        GIT_REPO_URL = "github.com/teamcloudyuga/kustomize-demo.git"
        GIT_REPO_EMAIL = 'cloudyuga@cloudyuga.guru'
        GIT_REPO_BRANCH = "9-putting-it-all-together-fixing-labels"
       // Update above variables with your user details
 
      }
      steps {
        container('tools') {
            sh "git clone https://$GIT_CREDS_USR:$GIT_CREDS_PSW@${env.GIT_REPO_URL}"
            sh "git config --global user.email ${env.GIT_REPO_EMAIL}"
 
          dir("kustomize-demo") {
              sh "git checkout ${env.GIT_REPO_BRANCH}"
              sh "cd ./overlays/staging && kustomize edit set image ${env.IMAGE_REPO}:${env.GIT_COMMIT}"
            sh "git commit -am 'Publish new version' && git push || echo 'no changes'"
          }
        }
      }
    }
 
    stage('Deploy to Prod') {
 
      steps {
        input message:'Approve deployment?'
        container('tools') {
          dir("kustomize-demo") {
              sh "cd ./overlays/prod && kustomize edit set image ${env.IMAGE_REPO}:${env.GIT_COMMIT}"
            sh "git commit -am 'Publish new version' && git push || echo 'no changes'"
          }
        }
      }
    }
  }
}
 
 
