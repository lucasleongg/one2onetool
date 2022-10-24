pipeline {
  agent any
  
  tools {nodejs "node"}
  
  environment {
        DATA_FILE = Questions-test.json
    }
  stages {
    stage('Example') {
      steps {
        sh 'npm config ls'
      }
    }
    stage('Install dependencies') {
      steps {
        sh 'npm install'
      }
    }
    stage('Test') {
      steps {
         sh 'npm test'
      }
    }
  }
}
