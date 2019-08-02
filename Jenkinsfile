pipeline {
  agent {
    docker {
      image 'wru_gradle:latest'
    }

  }
  stages {
    stage('example') {
      steps {
        sh 'image \'wru_gradle:latest\''
      }
    }
  }
}