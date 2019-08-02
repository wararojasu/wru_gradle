pipeline {
  agent any
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
    stage('post') {
      steps {
        sh 'touch /build/reports/tests/test/*.html'
      }
    }
  }
}