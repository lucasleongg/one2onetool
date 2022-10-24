pipeline {
  agent any
  
  tools {nodejs "node"}
  
  environment {
        DATA_FILE = "Questions-test.json"
        registry = "lucasleongg/govtech"
        registryCredential = 'dockerhub_id'
        dockerImage = ''
        container_name = "staging"
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
		try {
	    		sh "docker stop $container_name"
          		sh "docker rm $container_name"
		} catch (Exception e) {
      			echo 'Exception occurred: ' + e.toString()
      			echo 'Continue'
    		}
          	sh 'docker run -p --name $container_name 3000:3000 -d $registry:$BUILD_NUMBER'
        }
      }
    }
  }
}
