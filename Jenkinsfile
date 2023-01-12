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
          sh "docker build -t 192.168.100.12/shpark/sellers:rabbit-$BUILD_NUMBER ."
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
          sh "docker login 192.168.100.12 -u admin -p Unipoint11"
          sh "docker push 192.168.100.12/shpark/sellers:rabbit-$BUILD_NUMBER"
          }
        echo "Push Image END"
      }
    }
    

    stage('Deploy App') {
      steps {
        script {
          echo "Deploy App START"
          kubernetesDeploy configs: "rabbit_deployment.yaml", kubeconfigId: 'c7ec77d9-5b38-4554-9ec0-57aacac57f9b'
          sh "/usr/local/bin/kubectl --kubeconfig=/root/acloud-client.conf rollout restart deployment/rabbit -n default"
          echo "Deploy App END"
        }
      }
    }

  }

}
