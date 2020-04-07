def label = "worker-${UUID.randomUUID().toString()}"

podTemplate(label: label, containers: [
  containerTemplate(name: 'docker', image: 'docker:18.09', command: 'cat', ttyEnabled: true),
  containerTemplate(name: 'argo-cd', image: 'argoproj/argo-cd-ci-builder:v1.0.0', command: 'cat', ttyEnabled: true)
],
volumes: [
  hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock')
]) {
  node(label) {
    def myRepo = checkout scm
    def gitCommit = myRepo.GIT_COMMIT
    def gitBranch = myRepo.GIT_BRANCH
    def shortGitCommit = "${gitCommit[0..10]}"
    def previousGitCommit = sh(script: "git rev-parse ${gitCommit}~", returnStdout: true)

    stage('Create Docker images') {
      container('docker') {
        withCredentials([[$class: 'UsernamePasswordMultiBinding',
          credentialsId: 'dockerhub',
          usernameVariable: 'DOCKER_HUB_USER',
          passwordVariable: 'DOCKER_HUB_PASSWORD']]) {
          sh """
            docker login -u ${DOCKER_HUB_USER} -p ${DOCKER_HUB_PASSWORD}
            docker build -t nkhare/rsvp-demo:${gitCommit} .
            docker push nkhare/rsvp-demo:${gitCommit}
            """
        }
      }
    }
    stage('Deploy to Staging') {
      environment {
          GITREPO_URL = "https://github.com/nkhare/rsvpapp-kustomize"
          GITREPO_EMAIL = 'neependra.khare@gmail.com'
          GITREPO_BRANCH = "master" 
      }
      container('argo-cd') {
        withCredentials([[$class: 'UsernamePasswordMultiBinding',
          credentialsId: 'githubcred',
          usernameVariable: 'GITHUB_USER',
          passwordVariable: 'GITHUB_PASSWORD']]) {
          sh """
            echo "${env.GITREPO_URL}"
            echo "$$$$$$$$$$$"
            git clone https://$GITHUB_USER:$GITHUB_PASSWORD@${env.GITREPO_URL}
            git config --global user.email ${env.GITREPO_EMAIL}
            git checkout ${env.GITREPO_BRANCH}
            cd ./overlays/staging && kustomize edit set image ${env.IMAGE_REPO}:${env.GIT_COMMIT}
            git commit -am 'Publish new version' && git push || echo 'no changes'
             """
        }
      }
    }
  }
}
