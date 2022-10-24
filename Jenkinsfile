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
    stage('Build') {
      steps {
         sh 'npm install'
      }
    }
    stage('Test') {
      steps {
         sh 'npm test'
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Push Image') {
      steps {
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          sh 'docker run -p 3000:3000 -d $registry:$BUILD_NUMBER'
        }
      }
    }
  }
}
