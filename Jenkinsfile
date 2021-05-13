pipeline {
  agent any

  tools {
    jdk 'OpenJDK8'
    maven 'M3'
  }

  stages {
    stage('Build') {
      steps {
        withMaven(maven : 'M3') {
          sh "mvn package"
        }
      }
    }


    stage('Create and push container') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
          withMaven(maven : 'M3') {
            sh "mvn jib:build"
          }
        }
      } 
    }

    stage('Deploy to K8s') {
      steps {
        withKubeConfig([credentialsId: 'kubernetes-config']) {
          sh 'kubectl apply -f k8s.yaml'
        }
      } 
    }
  }
}
