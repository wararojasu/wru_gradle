pipeline {

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
  }
  post {
    always {
      deleteDir()
    }
  }  

}
