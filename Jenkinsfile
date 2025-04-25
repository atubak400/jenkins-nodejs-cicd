pipeline {
  agent any

  stages {
    stage('Clone Code') {
        steps {
            git branch: 'main', url: 'https://github.com/atubak400/jenkins-nodejs-cicd'
        }
    }

    stage('Install Dependencies') {
      steps {
        sh 'npm install'
      }
    }

    stage('Run Tests') {
      steps {
        sh 'npm test'
      }
    }

    stage('Build') {
      steps {
        sh 'echo "Build step - nothing to build"'
      }
    }

    stage('Deploy') {
      steps {
        sh 'nohup node index.js > output.log 2>&1 &'
      }
    }

    // stage('Docker Build & Push') {
    //     steps {
    //         script {
    //             dockerImage = docker.build("power3425/jenkins-nodejs-cicd")
    //             docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
    //             dockerImage.push()
    //             }
    //         }
    //     }
    // }
  }
}
