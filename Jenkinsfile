pipeline {
agent any
  environment {
      repository = '927086715722.dkr.ecr.us-west-2.amazonaws.com/test'
      tag = '0.1.1'
  }
  stages {
      stage('build'){
          steps {
            sh "aws ecr get-login --region us-west-2 | sed 's/-e none https:\\/\\///' | bash"
            sh "packer build -var 'repository=${repository}' -var 'tag=${tag}' packer.json"
            }
        }
       stage ('check'){

         steps {
              sh "trivy ${repository}:${tag}"  }
        }
 }}
