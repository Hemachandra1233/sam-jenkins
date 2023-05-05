pipeline {
  agent {
    kubernetes {
      yamlFile 'agent.yml'
    }
  }
 
  stages {
    
    
    
    stage('Install sam-cli') {
      steps {
        container('npm'){
          sh 'sam --version'
        }
      }
    }
    
    
    stage('Build') {
      steps {
        container('npm') {
          sh 'sam build'
          stash includes: '**/.aws-sam/**/*', name: 'aws-sam'
        }
      }
    }
    
    stage('prod') {
      environment {
        STACK_NAME = 'sam-app-prod-stage-example'
        S3_BUCKET = 'sam-jenkins-demo-ap-south-1-jenkins2'
      }
      steps {
        withAWS(credentials: 'sam-jenkins-example', region: 'ap-south-1') {
          unstash 'aws-sam'
          sh 'sam deploy --stack-name $STACK_NAME -t template.yaml --s3-bucket $S3_BUCKET --capabilities CAPABILITY_IAM'
        }
      }
    }
      

    
    
  }
}
