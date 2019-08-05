pipeline {
  environment {
    registry = "wararojasu/wru_gradle:first"
    registryCredential = 'AWT03_*123w'
    dockerImage = ''
  }
  
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '4'))
  }
  stages{
	stage('Build and Test'){
	   stages {
	      stage('Build') { 
	         steps {
	            sh './gradlew build'
	         }
	      }
	      stage('Test') { 
	         steps {
	               sh './gradlew test' 
	            }
	      }
       }
       post {
          always {
             junit 'build/test-results/test/*.xml'
             archiveArtifacts artifacts: 'build/libs/*.war', fingerprint: true
          }
       }
    }
	stage('Build, Public image to Docker Registry'){
	   stages {
	      stage('Build ..') { 
	        steps {
			    script {
			       dockerImage = docker.build registry
			    }
	        }
	      }
	      stage('Public ..') { 
	        steps {
			    script {
				   docker.withRegistry( '', 'docker-hub-credentials' ) {
				      sh 'docker login --username ${USERNAME} --password ${PASSWORD}'
				      dockerImage.push()
				   }				   
			    }
	        }
	      }
       }
    }	
  }
}
