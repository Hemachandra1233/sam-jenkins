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
}
