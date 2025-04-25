pipeline {
  agent any
  stages {
    stage('Clone Code') {
      steps {
        git 'https://github.com/your-user/jenkins-nodejs-cicd-demo.git'
      }
    }
    stage('Install Dependencies') {
      steps {
        sh 'npm install'
      }
    }
    stage('Run Tests') {
      steps {
        sh 'echo "No tests yet"'
      }
    }
    stage('Build') {
      steps {
        sh 'echo "Build step - nothing to build"'
      }
    }
    stage('Deploy') {
      steps {
        sh 'node index.js &'
      }
    }
  }
}
