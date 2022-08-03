def  rtMaven = Artifactory.newMavenBuild()


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
	
	stage("Setup Artifactory Repo") {
	  steps {
	    def  server = Artifactory.server "ssoella-artifactory"
	    script {
		  rtMaven.tool = "my_maven"
		  rtMaven.deployer releaseRepo: "initializr", server: server
	    }
	  }
	}
	
	stage("Deploy Artifact to Artifactory Repo") {
	  steps {
	    script {
		  rtMaven.run pom: "pom.xml", goals: "clean install", buildInfo: buildInfo
		  
	    }
	  }	
	}
		 
  }
}
	

