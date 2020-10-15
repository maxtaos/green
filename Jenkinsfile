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
    - name: dockersock
      mountPath: /var/run/docker.sock
  - name: gcloud
    image: gcr.io/cloud-builders/gcloud
    command:
    - cat
    tty: true
  volumes:
  - name: dockersock
    hostPath:
      path: /var/run/docker.sock
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
