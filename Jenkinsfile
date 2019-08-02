pipeline {
  agent {
    docker {
      image 'wru_gradle'
    }

  }
  stages {
    stage('listar') {
      steps {
        sh 'ls -l'
      }
    }
    stage('test') {
      steps {
        sh 'ls'
      }
    }
  }
}