def  rtMaven = Artifactory.newMavenBuild()
def  rtMaven.deployer server: server, releaseRepo: 'initializr'

pipeline {
  agent any
  tools {
     maven "my_maven"
	 rtMaven.tool = "my_maven"
	 rtMaven.deployer server: server, releaseRepo: 'initializr'
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
	    def buildInfo = rtMaven.run pom: 'maven-example/pom.xml', goals: 'clean install'
	  }
	  
	}
	
		 
  }
}
	

