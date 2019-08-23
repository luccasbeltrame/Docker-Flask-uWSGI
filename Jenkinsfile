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
    stage('Apply Kubernetes files') {
      steps{
          kubernetesDeploy(
             kubeconfigId: 'kubeconfig'
          ) 
         sh ("kubectl get po")
    }
  }
}
}
