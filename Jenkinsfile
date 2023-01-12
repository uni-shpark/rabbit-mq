pipeline {

  environment {
    registry = "uni-shpark/rabbit-mq"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        echo "Checkout Source START"
        git 'https://github.com/uni-shpark/rabbit-mq.git'
        echo "Checkout Source END"
      }
    }

    stage('Build image') {
      steps{
        script {
          echo "Build image START $BUILD_NUMBER"
          sh "docker shpark/sellers:rabbit-$BUILD_NUMBER"
          dockerImage = docker.build("192.168.100.12/shpark/sellers:rabbit-$BUILD_NUMBER")
          echo "Build image END"
        }
      }
    }

    stage('Push Image') {
      environment {
               registryCredential = 'harbor'
           }
      steps{
        script {
          echo "Push Image START"
          sh "docker push 192.168.100.12/shpark/sellers:rabbit-$BUILD_NUMBER"
          }
        echo "Push Image END"
      }
    }
    

    stage('Deploy App') {
      steps {
        script {
          echo "Deploy App START"
          kubernetesDeploy(configs: "rabbit_deployment.yaml", kubeconfigId: "c7ec77d9-5b38-4554-9ec0-57aacac57f9b")
          echo "Deploy App END"
        }
      }
    }

  }

}
