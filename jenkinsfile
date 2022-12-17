pipeline {
  agent any
  tools {
    maven "maven-3.8.6"
    
  
  }
  stages {
    stage('Build Application') {
      steps {
        sh 'mvn clean package'
      
      }
      post {
        success {
          echo "Stating the archive process"
          archiveArtifacts artifacts: '**/*.war'
        
        }
      
      }
      
    
    }
    stage ('Deploy Application'){
        steps {
          build job: 'APPLICATION-DEPLOYMENT-JOB'
        }
      
      }
  
  }
  
  
  
  
}
