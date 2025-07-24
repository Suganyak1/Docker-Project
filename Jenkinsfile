pipeline {

  environment {
    registry = "docker.io/suganyak12/flask"
    registry_mysql = "docker.io/suganyak12/mysql"
    dockerImage = ""
  }

  agent any
    stages {
  
    stage('Checkout Source') {
      steps {
        git 'https://github.com/mgsgoms/Docker-Project.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
            dockerImage.push()
          }
        }
      }
    }

    stage('current') {
      steps{
        dir("${env.WORKSPACE}/mysql"){
          sh "pwd"
          }
      }
   }
   stage('Build mysql image') {
     steps{
       sh 'docker build -t "docker.io/cubensquare/mysql:$BUILD_NUMBER"  "$WORKSPACE"/mysql'
        sh 'docker push "docker.io/cubensquare/mysql:$BUILD_NUMBER"'
        }
      }
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "frontend.yaml", kubeconfigId: "kube")
        }
      }
    }

  }

}
