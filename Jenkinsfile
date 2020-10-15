pipeline {
  agent {
    kubernetes {
      label 'image-builddd'
      idleMinutes 10
      defaultContainer 'jnlp'
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
    command: ['cat  ']
    tty: true
    volumeMounts:
      - mountPath: "/root/.m2"
        name: m2
  - name: docker
    image: docker:1.11
    command: ['cat']
    tty: true
    volumeMounts:
    - mountPath: /var/run/docker.sock
      name: docker-sock
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
