pipeline {
  agent any
  environment {
    IMAGE = "myapp"
    TAG   = "${BUILD_NUMBER}"
  }
  stages {
    stage('Checkout') {
      steps { git branch: 'main', url: 'https://github.com/USER/REPO.git' }
    }
    stage('Build Image') {
      steps { sh 'docker build -t $IMAGE:$TAG .' }
    }
    stage('Test') {
      steps { sh 'docker run --rm $IMAGE:$TAG python -c "import flask; print(1)"' }
    }
    stage('Deploy Container') {
      steps {
        sh 'docker rm -f myapp || true'
        sh 'docker run -d --name myapp -p 5000:5000 $IMAGE:$TAG'
      }
    }
  }
  post {
    always  { sh 'docker ps' }
    failure { echo 'Build fail ho gaya!' }
  }
}
