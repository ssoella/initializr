pipeline {
  agent any
  tools {
     maven "my_maven"
	 
  }

  stages {
    stage("GIT Checkout") {
      steps {
	    git url: "https://github.com/ssoella/initializr.git"
	  }
	 
    }
	
	stage("Compile") {
	  steps {
	    sh "mvn compile"
	    
	  }
		
	}
	
	stage("SonarQube Analysis") {
	   steps {
	      script {
            scannerHome = tool "my_sonarqube";
		  }
          withSonarQubeEnv("mysonarserver") {
          sh "${scannerHome}/bin/sonar-scanner"
          } 
       }  
	}
	 
  }
}
	

