pipeline { 
    environment {
    registry = "sitaramreddy/k8s-jenkins-demo"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }  
  agent any  
  stages {
      stage('Cloning Git') {
          steps {
             git 'https://github.com/sitaram2423/demo-node.git'
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
    stage('deploying to k8s-cluster') {
      steps{
        script {
           kubernetesDeploy {
                    configs: 'k8s-deploy.yml',
                    kubeconfigId: 'kubeconfig',
                    enableConfigSubstitution: 'true'
    }
   }
  }
 }
}
}
