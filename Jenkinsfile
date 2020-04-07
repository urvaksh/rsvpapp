pipeline {
    agent {
      kubernetes  {
            label 'jenkins-slave'
        }
    }
  environment {
      IMAGE_REPO = "nkhare/rsvp-demo"
  }
  stages {
    
    stage('Build') {
      environment {
        DOCKERHUB_CREDS = credentials('dockerhub')
        
      }
      container('docker') {
          // Build new image
        sh """
           pwd
           docker image ls 
           """
      }
    }
  }
}
