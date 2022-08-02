def  rtMaven = Artifactory.newMavenBuild()
def  server = Artifactory.server

pipeline {
  agent any
  tools {
     maven "my_maven"
	 
  }

  stages {
    	
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
	
	stage("Deploy Artifact to Artifactory Repo") {
	  steps {
	    script {
		  rtMaven.tool = "my_maven"
	      rtMaven.deployer.deployArtifacts  buildInfo 
		  rtMaven.deployer releaseRepo: 'initializr', server: server
	    }
	  }
	}
	
		 
  }
}
	

