pipeline {
      agent any
	  
	  tools {
	      maven "Maven 3.9.0"
		  
		}
		
      stages {
            stage('Build Application') {
                  steps {
                        sh 'mvn clean install'
                    
                  }
            
            post {
			    success {
                    echo "Strating the archive process"
					archiveArtifacts artifacts: '**/*.war'
					
		}
	    }
				   
	}
    		                        
      }
}
