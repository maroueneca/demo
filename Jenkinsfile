pipeline {

  environment {
    registry = "172.42.42.100:5000/justme/myweb"  
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
          echo 'build image'
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
          
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

