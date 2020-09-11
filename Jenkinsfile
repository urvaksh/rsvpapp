pipeline {
environment {
    IMAGE_REPO = "nkhare/rsvpapp"
    // Instead of nkhare, use your git username
}
stages {
  stage('Build') {
    environment {
      DOCKERHUB_CREDS = credentials('dockerhub')
    }
    steps {
      container('docker') {
        // Build new image
        sh "until docker container ls; do sleep 3; done && docker image build -t  ${env.IMAGE_REPO}:${env.GIT_COMMIT} ."
        // Publish new image
        sh "docker login --username $DOCKERHUB_CREDS_USR --password $DOCKERHUB_CREDS_PSW && docker image push ${env.IMAGE_REPO}:${env.GIT_COMMIT}"
      }
    }
  }
 }
}
