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
      
   
    
    
    
    
  }
}
