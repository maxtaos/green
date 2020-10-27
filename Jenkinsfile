pipeline {
agent any
  stages {
      stage ('init'){
          steps {
              def repository = 'roman610/nginx-test'
              def tag = '0.1.1'       }
        }
      stage('build'){
          steps {
            sh "aws ecr get-login --region us-west-2 | sed 's/-e none https:\\/\\///' | bash"
            sh "packer build -var 'repository=$repository' -var 'tag=$tag' /tmp/docktest.json"    }
        }
       stage ('check'){
          steps {
              sh "trivy $repository:$tag"  }
        }
 }}
