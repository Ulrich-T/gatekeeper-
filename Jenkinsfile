pipeline {
  agent any 
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    //HEROKU_API_KEY = credentials('darinpope-heroku-api-key')
     DOCKERHUB_CREDENTIALS=credentials('dockerhub')
  }
  parameters { 
    string(name: 'APP_NAME', defaultValue: '', description: 'What is the Raul app name?') 
  }
  stages {
    stage('Build') {
      steps {
        sh 'docker build -t darinpope/java-web-app:latest .'
      }
    }
    stage('Login') {
      steps {
       // sh 'echo $HEROKU_API_KEY | docker login --username=_ --password-stdin registry.heroku.com'
       sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push to raul012 registry') {
      steps {
        
      /*  sh '''
          docker tag darinpope/java-web-app:latest registry.heroku.com/$APP_NAME/web
          docker push registry.heroku.com/$APP_NAME/web
        ''' */
        
         sh '''
          docker tag darinpope/java-web-app:latest raul012/$APP_NAME/web
          docker push raul012/$APP_NAME/web
        '''
        
      }
    }
 /*   stage('Release the image') {
      steps {
        sh '''
          heroku container:release web --app=$APP_NAME
        '''
      }
    }
  } */
  post {
    always {
      sh 'docker logout'
    }
  }
}
