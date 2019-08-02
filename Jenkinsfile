pipeline {
  agent {
    docker {
      image 'wru_gradle'
      args ':latest'
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