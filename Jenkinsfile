pipeline {

  environment {
    dockerimagename = "static92/lol"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/static92/test3.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhub-credentials'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("puk")
          }
        }
      }
    }

    stage('Deploying React.js container to Kubernetes') {
      steps {
        withKubeConfig([credentialsId: 'kube', serverUrl: 'https://192.168.0.116:6443']) {
          sh 'kubectl apply -f deployment.yaml'
          sh 'kubectl apply -f service.yaml'
        }
      }
    }

  }

}

