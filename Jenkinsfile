pipeline {
  agent any
  tools {
      maven 'maven'
    }
  stages {
    stage('Build') {
      steps {
        sh 'mvn clean install'
        sh 'mvn package'
        sh 'docker build -t skymapled/simple-spring:${BUILD_NUMBER} .'
      }
    }
    stage('Run Docker image') {
          steps {
            sh 'docker run --name test -d -p 8888:8080 skymapled/simple-spring:${BUILD_NUMBER}'
          }
        }
    stage('Test') {
      steps {
        sh 'curl http://127.0.0.1:8888/swagger-ui/index.html'
      }
    }
    stage('Stop Docker container') {
          steps {
            sh 'docker rm test -f'
          }
        }
    stage('Push docker image') {
      steps {
        sh 'docker push skymapled/simple-spring:${BUILD_NUMBER}'
      }
    }
    stage('Trigger ManifestUpdate') {
    steps{
                    build job: 'updatemanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
            }
            }
  }
}