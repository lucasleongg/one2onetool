pipeline {
  agent any
  
  tools {nodejs "node"}
  
  environment {
        DATA_FILE = "Questions-test.json"
        registry = "lucasleongg/govtech"
        registryCredential = 'dockerhub_id'
        dockerImage = ''
    }
  stages {
//     stage('Test') {
//       steps {
//          sh 'npm install'
//       }
//     }
//     stage('Test') {
//       steps {
//          sh 'npm test'
//       }
//     }
    stage('list') {
      steps{
          sh 'ls'
          sh 'pwd'
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
  }
}
