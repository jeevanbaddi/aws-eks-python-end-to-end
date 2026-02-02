pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t python-jenkins-test .'
      }
    }

    stage('Run Container Test') {
      steps {
        sh '''
          docker run -d -p 5001:5000 --name python-test python-jenkins-test
          sleep 5
          curl http://localhost:5001
        '''
      }
    }
  }

  post {
    always {
      sh 'docker rm -f python-test || true'
    }
  }
}
