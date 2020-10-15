pipeline {
  agent {
    kubernetes {
      label 'image-build'
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
    command:
    - cat
    tty: true
  - name: gcloud
    image: gcr.io/cloud-builders/gcloud
    command:
    - cat
    tty: true
"""
}
  }
  stages {
    stage('Build Images') {
      parallel {
        stage('Build GCE Image with Packer') {
          steps {
            container('packer') {
              sh """
                python -m pip install ansible
                packer build packer.json
              """
            }
          }
        }
        stage('Build and push image with Cloud Build') {
          steps {
            container('gcloud') {
              sh """
              PROJECT_ID=`curl -s 'http://metadata/computeMetadata/v1/project/project-id' -H 'Metadata-Flavor: Google'`
              BRANCH_NAME=${GIT_BRANCH}
              IMAGE_TAG=gcr.io/\${PROJECT_ID}/redmine:\${BRANCH_NAME#*/}-${GIT_COMMIT}
              PYTHONUNBUFFERED=1 gcloud builds submit -t \${IMAGE_TAG} .
              """
            }
          }
        }
      }
    }
  }
}
