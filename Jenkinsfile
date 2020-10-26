pipeline {
  agent any
  stages {
  stage ('build'){
    def repository = 'roman610/nginx-test'
    def tag = '0.1.1'
    sh "packer build -var 'repository=$repository' -var 'tag=$tag' /tmp/docktest.json"
  }
   stage ('check'){
   sh "trivy $repository:$tag"
  }
 }
}
