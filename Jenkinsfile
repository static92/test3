pipeline {

  environment {
    dockerimagename = "static92/lol"
    dockerImage = ""
  }

   agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          containers:
          - name: docker
            image: docker:latest
            command:
            - cat
            tty: true
            volumeMounts:
             - mountPath: /var/run/docker.sock
               name: docker-sock
          volumes:
          - name: docker-sock
            hostPath:
              path: /var/run/docker.sock    
        '''
    }
   }

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/Bravinsimiyu/jenkins-kubernetes-deployment.git'
      }
    }

    stage('Build image') {
      steps{
        container('docker') {
          sh 'docker build -t static92/lol:kekus .'
        }
      }
    }
    stage('Login-Into-Docker') {
      steps {
        container('docker') {
          sh 'docker login -u static92 -p W7xryrfs92'
      }
    }
    }
    stage('Pushing Image') {
      steps{
        container('docker') {
          sh 'docker push static92/lol:kekus' 
          }
      }
    }
    stage('Apply Kubernetes files') {
      steps{
        withKubeConfig([credentialsId: 'kube', serverUrl: 'https://192.168.0.116']) {
          sh 'kubectl apply -f deployment.yaml'
    }
      }
    }
  }

}
