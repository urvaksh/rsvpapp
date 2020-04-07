pipeline {
        agent {
                kubernetes  {
                        label 'jenkins-slave'
                }
        }
      
        stages {

                        stage('Build') {

                                steps {
                                        container('docker1') {
                                               sh """
                                                  date
                                                  """
                                                // Build new image
                                                //sh "until docker ps; do sleep 3; done && docker build -t  ${env.IMAGE_REPO}:${env.GIT_COMMIT} ."
                                                        // Publish new image
                                                //sh "docker login --username $DOCKERHUB_CREDS_USR --password $DOCKERHUB_CREDS_PSW && docker push ${env.IMAGE_REPO}:${env.GIT_COMMIT}"
                                        }
                                }
                        }
        }
}
