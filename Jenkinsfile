pipeline {
  agent {
    kubernetes {
      label 'image-build33'
      idleMinutes 10
      defaultContainer 'jnlp33'
      yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    component: ci
spec:
  # Use service account that can deploy to all namespaces
  serviceAccountName: cd-jenkins
  containers:
  - name: packer
    image: hashicorp/packer:full
    command:
    - cat
    tty: true
    volumeMounts:
      - mountPath: "/root/.m2"
        name: m2
  - name: docker
    image: docker:latest
    command:
    - cat
    tty: true
    volumeMounts:
    - mountPath: /var/run/docker.sock
      name: docker-sock
  - name: gcloud
    image: gcr.io/cloud-builders/gcloud
    command:
    - cat
    tty: true
  volumes:
    - name: docker-sock
      hostPath:
        path: /var/run/docker.sock
    - name: m2
      persistentVolumeClaim:
        claimName: m2
"""
}
  }
  stages {
    stage('Build Images') {
          steps {
            container('packer') {
              sh """
                packer build packer.json
              """
            }
          }
    }
  }
}
