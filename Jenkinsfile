pipeline {
  agent {
    kubernetes {
      label 'image-build-5'
      idleMinutes 10
      defaultContainer 'jnlp-5'
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
    env:
      - name: DOCKER_HOST
        value: tcp://localhost:2375
  - name: dind
    image: docker:18.05-dind
    securityContext:
      privileged: true
    volumeMounts:
      - name: dind-storage
        mountPath: /var/lib/docker
  - name: gcloud
    image: gcr.io/cloud-builders/gcloud
    command:
    - cat
    tty: true
  volumes:
  - name: dind-storage
    emptyDir: {}
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
