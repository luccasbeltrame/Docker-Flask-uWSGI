pipeline {
  environment {
    registry = "luccasbeltrame/app"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/luccasbeltrame/Docker-Flask-uWSGI.git'
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
    stage('deploy kubernetes') {
      steps{
        sh "kubectl apply -f https://raw.githubusercontent.com/luccasbeltrame/Docker-Flask-uWSGI/master/k8s_app.yaml"
        sh "kubectl set image deployment app app=${dockerImage} --record"
        sh "kubectl rollout status deployment/app"  
    }
  }
}
}
