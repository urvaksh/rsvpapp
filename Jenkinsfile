pipeline {
        agent {
                kubernetes  {
                        label 'jenkins-slave'
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
                                                sh "echo '$$$$$$$$'"
                                                // Build new image
                                                //sh "until docker ps; do sleep 3; done && docker build -t  ${env.IMAGE_REPO}:${env.GIT_COMMIT} ."
                                                        // Publish new image
                                                //sh "docker login --username $DOCKERHUB_CREDS_USR --password $DOCKERHUB_CREDS_PSW && docker push ${env.IMAGE_REPO}:${env.GIT_COMMIT}"
                                        }
                                }
                        }
        }
}
