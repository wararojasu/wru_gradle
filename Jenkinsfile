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
				      dockerImage.push()
				   }				   
			    }
	        }
	      }
       }
    }
	stage('Pull and Deploy ...'){
	   stages {
	      stage('Pull ..') { 
	        steps {
			    script {
				   docker.withRegistry('', 'docker-hub-credentials') {
					  dockerImage = docker.image('wararojasu/wru_gradle:first')
					  dockerImage.pull()
				  }				   
			    }
	        }
	      }
		  stage('run-parallel-branches') {
		    steps {
			  parallel(
			    Qa: {
				  sh 'docker run --name container-qa -d -p 8787:8080 wararojasu/wru_gradle:first'
			    },
			    Dev: {
				  sh 'docker run --name container-dev -d -p 8785:8080 wararojasu/wru_gradle:first' 
			    }
			  )
		    }
		  }		  
       }
    }
	stage('Build and Test GUI automation ...'){
	   stages {
          stage('Checkout external gui test ...') {
            steps {
				sh 'git clone -b master https://github.com/wararojasu/wru_gradle_gui_test.git' 
            }
          }	   
	      stage('Build GUI tes') { 
	         steps {
			    sh 'cd wru_gradle_gui_test'
	            sh './gradlew build'
	         }
	      }
	      stage('Test GUI test') { 
	         steps {
	               sh './gradlew test' 
	            }
	      }
       }
       post {
          always {
             junit 'build/test-results/test/*.xml'
          }
		 success {
		  sh 'echo "This will run only if successful"'
		  emailext attachmentsPattern: 'build/reports/tests/test/index.html', mimeType: 'text/html', body: '''${SCRIPT, template="groovy-html.template"}''', subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - Successful", to: 'wara.rojas.u@gmail.com'
		 }
		 failure {
		  sh 'echo "This will run only if failed"'
		 }
		 unstable {
		  sh 'echo "This will run only if the run was marked as unstable"'
		 }
		 changed {
		  sh 'echo "This will run only if the state of the Pipeline has changed"'
		  sh 'echo "For example, the Pipeline was previously failing but is now successful"'
		  sh 'echo "... or the other way around :)"'
		 }			  
       }	   
    }
  }
}
