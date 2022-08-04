def  rtMaven = Artifactory.newMavenBuild()
def buildInfo

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
	    script {
		  def  server = Artifactory.server "ssoella-artifactory"
		  buildInfo = Artifactory.newBuildInfo()
		  rtMaven.tool = "my_maven"
		  rtMaven.deployer server: server, releaseRepo: "initializr", snapshotRepo: "initializr-snapshot"
		  buildInfo.env.capture = true
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
   
	post("Notify Slack Channel") {
	   success {
	     slackSend message: "Build Successful - ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
	   }
	}	 
  
}
	

