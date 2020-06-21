pipeline {

  environment {
    registry = "172.42.42.100:5000/justme/myweb"
    path="demo/."
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/maroueneMM/demo.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          sh "echo "${dockerImage} ${registry} ${BUILD_NUMBER} ${path}""
          dockerImage = docker.build registry + ":$BUILD_NUMBER" path
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( "" ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}
