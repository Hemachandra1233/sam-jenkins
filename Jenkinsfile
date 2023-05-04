pipeline {

  agent {
    kubernetes {
      yamlFile 'agent.yml'
    }
  }
  environment {
        AWS_ACCESS_KEY_ID = credentials('aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
        AWS_DEFAULT_REGION = 'ap-south-1'
        LAMBDA_FUNCTION_NAME = 'jenkinsLambdaDeployment'
    }

  stages {
  
    stage('Install sam-cli') {
      steps {
        container('python') {
          sh 'python3 -m venv venv && venv/bin/pip install aws-sam-cli'
		  stash includes: '**/venv/**/*', name: 'venv'
        }
      }
    }
	  
    stage('Build') {
      steps {
	container('python') {      
	  unstash 'venv'
	  sh 'venv/bin/sam build'
	  stash includes: '**/.aws-sam/**/*', name: 'aws-sam'
        }
      }
    }
    
    stage('beta') {
      environment {
        STACK_NAME = 'sam-app-beta-stage'
        S3_BUCKET = 'sam-jenkins-demo-ap-south-1-jenkins'
      }
      steps {
	container('python'){
	  withAWS(credentials: 'sam-jenkins-example', region: 'ap-south-1') {
            unstash 'venv'
            unstash 'aws-sam'
            sh 'venv/bin/sam deploy --stack-name $STACK_NAME -t template.yaml --s3-bucket $S3_BUCKET --capabilities CAPABILITY_IAM'
            dir ('hello-world') {
              sh 'npm ci'
              sh 'npm run integ-test'
            }
          }
	} 
      }
    }	  
	  
	  
	  
	  
  }
}
