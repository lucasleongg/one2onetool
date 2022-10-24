pipeline {
  agent any
  
  tools {nodejs "node"}
  
  environment {
        DATA_FILE = "Questions-test.json"
        registry = "lucasleongg/govtech"
        registryCredential = 'dockerhub_id'
        dockerImage = ''
//         container_name = "staging"
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
		dockerImage = docker.build registry + "/${env.BRANCH_NAME}:$BUILD_NUMBER"
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
	    		sh "docker stop ${env.BRANCH_NAME}"
          		sh "docker rm ${env.BRANCH_NAME}"
		} catch (Exception e) {
      			echo 'Exception occurred: ' + e.toString()
      			echo 'Continue'
    		}
          	sh 'docker run --name ${env.BRANCH_NAME} -p 3000:3000 -d $registry_env/${env.BRANCH_NAME}:$BUILD_NUMBER'
        }
      }
    }
  }
}
