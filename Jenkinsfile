pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'mvn clean install'
        sh 'mvn package'
        sh 'docker build -t skymapled/jenkins-spring-build .'
      }
    }
    stage('Test') {
      steps {
        echo "Testing..."
        sh 'mvn test'
      }
    }
    stage('Deploy') {
      steps {
        sh 'docker run -d -p 8888:8080 skymapled/jenkins-spring-build'
      }
    }
  }
}
