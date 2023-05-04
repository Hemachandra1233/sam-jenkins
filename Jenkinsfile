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
      
   
    
    
    
    
  }
}
