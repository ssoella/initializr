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
	
	stage("Maven Compile") {
	  steps {
	    sh "mvn compile"
	    
	  }
		
	}
	
		
	stage ("Maven Clean") {
	  steps {
	    sh "mvn clean package"
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
	

