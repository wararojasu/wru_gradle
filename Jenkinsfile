pipeline {
  agent {
    docker {
      image 'wru_gradle'
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