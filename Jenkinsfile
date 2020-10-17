pipeline {
  agent {
    kubernetes {
      label 'image-build-ff'
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
  volumes:
  - type: HostPath
    hostPath: /var/run/docker.sock
    mountPath: /var/run/docker.sock
"""
}
  }
  stages {
    stage('Build Images') {
          steps {
            container('packer') {
              sh """
                docker version
                packer build packer.json
              """
            }
          }
    }
  }
}
