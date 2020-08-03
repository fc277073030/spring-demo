pipeline {
  agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    some-label: some-label-value
spec:
  containers:
  - name: docker
    image: docker:stable
    command:
    - cat
    tty: true
    volumeMounts:
      - mountPath: /var/run/docker.sock
        name: docker
  - name: maven
    image: maven:alpine
    command:
    - cat
    tty: true
    volumeMounts:
      - mountPath: "/root/.m2"
        name: maven-m2
  volumes:
    - name: docker
      hostPath:
        path: /var/run/docker.sock
    - name: maven-m2
      persistentVolumeClaim:
        claimName: maven-m2

"""
    }
  }
  stages {
    stage('Build') {
      steps {
        container('maven') {
          sh 'mvn install'
          sh 'ls -l spring-demo'
        }
        container('docker') {
          sh 'docker ps'
        }
      }
    }
  }
}
