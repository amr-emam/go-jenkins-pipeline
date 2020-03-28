pipeline {
  agent {
    kubernetes {
      label 'jenkins-agent'
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
labels:
  component: ci
spec:
  # Use service account that can deploy to all namespaces
  serviceAccountName: jenkins
  containers:
  - name: maven
    image: maven:latest
    command:
    - cat
    tty: true
  - name: docker
    image: docker:latest
    command:
    - cat
    tty: true
    volumeMounts:
    - mountPath: /var/run/docker.sock
      name: docker-sock
  volumes:
    - name: docker-sock
      hostPath:
        path: /var/run/docker.sock
"""
}
   }
  stages {
    stage('Build') {
      steps {
        container('maven') {
          sh """
                        echo "maven build";
                                                """
        }
      }
    }
    stage('Test') {
      steps {
        container('maven') {
          sh """
             echo "maven test";
          """
        }
      }
    }
    stage('Push') {
      steps {
        container('docker') {
          sh """
             docker build -t spring-petclinic-demo:$BUILD_NUMBER .
          """
        }
      }
    }
  }
}