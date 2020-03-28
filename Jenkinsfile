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
  serviceAccount: jenkins
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
  - name: kubectl
    image: bitnami/kubectl:latest
    command:
    - cat
    tty: true
"""
}
   }
  stages {
    stage('Build') {
      steps {
        container('maven') {
          sh """
            pwd;
            ls -ltr;
            echo "maven build" > /home/jenkins/agent/from-maven-image;
             """
        }
      }
    }
    stage('Test') {
      steps {
        container('docker') {
          sh """
            pwd;
            ls -ltr;
            ls -ltr /home/jenkins/agent/;
            """
        }
      }
    }
    stage('Push') {
      steps {
        container('docker') {
          sh """
             echo "build";
            """
        }
      }
    }
    stage('Deploy') {
      steps {
        container('kubectl') {
          sh """
             kubectl get pods;
            """
        }
      }
    }
  }
}