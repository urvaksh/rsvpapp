pipeline {
  agent {
    kubernetes  {
          label 'jenkins-slave'
          defaultContainer 'jnlp'
    yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: dind
    image: docker:18.09-dind
    securityContext:
      privileged: true
  - name: docker
    env:
    - name: DOCKER_HOST
      value: 127.0.0.1
    image: docker:18.09
    command:
    - cat
    tty: true
"""
      }
  }
environment {
    IMAGE_REPO = "nkhare/rsvp-demo"
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
 }
}
