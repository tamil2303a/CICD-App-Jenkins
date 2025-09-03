pipeline {
  agent any
  environment {
    APP_NAME = "flask-app"
    APP_PORT = "5000"
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t ${APP_NAME}:latest .'
      }
    }
    stage('Deploy') {
      steps {
        sh '''
          docker stop ${APP_NAME} || true
          docker rm ${APP_NAME} || true
          docker run -d --name ${APP_NAME} -p ${APP_PORT}:5000 ${APP_NAME}:latest
        '''
      }
    }
  }
}
