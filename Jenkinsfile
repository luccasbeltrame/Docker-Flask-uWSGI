node {

    checkout scm

    // Pega o commit id para ser usado de tag (versionamento) na imagem
    sh "git rev-parse --short HEAD > commit-id"
    tag = readFile('commit-id').replace("\n", "").replace("\r", "")
    
  stage('Build image') {
    dockerImage = docker.build("luccasbeltrame/app:latest")
  }

  stage('Push image') {
    docker.withRegistry('https://registry-1.docker.io/v2/', 'dockerhub') {
      dockerImage.push()
    }
  }

}

    stage "Deploy PROD"

        input "Deploy to PROD?"
        DockerImage.push('latest')
        sh "kubectl apply -f https://raw.githubusercontent.com/luccasbeltrame/Docker-Flask-uWSGI/master/k8s_app.yaml"
        sh "kubectl set image deployment app app=${imageName} --record"
        sh "kubectl rollout status deployment/app"
}
