pipeline {
  agent {
    kubernetes {
      label 'image-builda'
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
    - name: jenkins-home
      mountPath: /var/jenkins_home/
    - mountPath: /var/run/docker.sock
      name: docker-sock-volume
  volumes:
  - name: jenkins-home
  - name: docker-sock-volume
    hostPath:
      path: /var/run/docker.sock
      type: File

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
